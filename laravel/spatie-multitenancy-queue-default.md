# Spatie multitenancy: set `queues_are_tenant_aware_by_default` to `false` for landlord-scoped jobs

`spatie/laravel-multitenancy` defaults `queues_are_tenant_aware_by_default => true`. That means every queued job expects a "current tenant" to be set when it runs.

If you dispatch a job from a **public / landlord** context (e.g. a marketing-site signup pipeline that provisions a new tenant), the worker throws:

```
CurrentTenantCouldNotBeDeterminedInTenantAwareJob
```

…and the job dies. Symptom I hit: every new signup hung forever at "analyzing" because the provisioning chain silently failed in the worker.

## Fix

For products where most work is landlord-scoped, flip the default in `config/multitenancy.php`:

```php
'queues_are_tenant_aware_by_default' => false,
```

Then opt **in** per job only where you genuinely need tenant context, by implementing Spatie's `TenantAware` interface on that job.

## Takeaway

Match the default to your app's center of gravity. Landlord-heavy app → tenant-aware off by default, opt in deliberately. Tenant-heavy app → leave it on.
