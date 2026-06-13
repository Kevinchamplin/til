# WPGraphQL returns ACF `select` fields as an array

An Advanced Custom Fields **select** field (even a single-value, non-multiple one) comes back from WPGraphQL as a *list of strings*, not a scalar string.

So this returns `null` at render time:

```ts
const type = project.projectFields.projectType; // ["SaaS Platform"] — an array!
```

You have to read the first element:

```ts
const type = project.projectFields.projectType?.[0] ?? null;
```

The same is true for other ACF "choice" fields exposed to GraphQL (e.g. a `status` select). I've been bitten by assuming the multiple-vs-single toggle in the ACF UI changes the GraphQL shape — it doesn't reliably; treat select output as `string[]`.

## Why

ACF stores select values in a way that maps cleanly to a list, and the WPGraphQL ACF integration surfaces that list type rather than collapsing single selections to a scalar.

## Takeaway

For ACF `select` over WPGraphQL, type it as `string[]` and read `[0]`. Don't trust the field to be a bare string.
