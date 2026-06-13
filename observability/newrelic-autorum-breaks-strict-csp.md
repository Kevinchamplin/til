# New Relic auto-RUM injects inline scripts and breaks strict CSP

A site with a strict Content-Security-Policy (no `unsafe-inline`) suddenly logs CSP violations and the browser blocks scripts you never wrote. Cause: the New Relic PHP agent's **auto-RUM (Real User Monitoring)** feature rewrites the HTML response on the way out and injects an inline `<script>` (the browser timing beacon) into every page.

That inline script has no nonce/hash, so a strict CSP correctly refuses it — tanking your Lighthouse "Best Practices" score and breaking any "no third-party inline scripts" guarantee.

## Fix

Disable auto-RUM where you control output. Per-page, early in the bootstrap:

```php
if (function_exists('newrelic_disable_autorum')) {
    newrelic_disable_autorum();
}
```

Or globally via INI: `newrelic.browser_monitoring.auto_instrument = false`. If you still want RUM, inject the header/footer snippets yourself with a proper nonce that your CSP allows.

## Takeaway

If a "phantom" inline script is violating your CSP on a New Relic host, it's auto-RUM rewriting the response. Disable `auto_instrument` (or call `newrelic_disable_autorum()`), then add RUM back with a nonce if you want it.
