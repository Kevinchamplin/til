# Tag-based ISR revalidation from a CMS webhook

For a headless CMS + Next.js App Router site, time-based ISR (`revalidate: 3600`) means content is stale for up to an hour. Tag-based revalidation makes edits go live on demand instead.

Tag your fetches by content type:

```ts
await fetch(endpoint, { next: { tags: ['services', `page-${slug}`] } });
// or unstable_cache(fn, keys, { tags: ['projects'] })
```

Expose a webhook the CMS calls on save:

```ts
// app/api/revalidate/route.ts
import { revalidateTag } from 'next/cache';

export async function POST(req: Request) {
  const { searchParams } = new URL(req.url);
  if (searchParams.get('secret') !== process.env.REVALIDATION_SECRET) {
    return new Response('Unauthorized', { status: 401 });
  }
  revalidateTag(searchParams.get('tag') ?? '');
  return Response.json({ revalidated: true });
}
```

## Gotcha

The tag string in the webhook must **exactly** match the tag on the fetch. A typo means the call returns `200` but nothing actually rebuilds — so it looks like it worked. Keep tags in shared constants.

## Takeaway

Tag every CMS fetch, revalidate by tag from an authenticated webhook, and centralize the tag names so producer and consumer can't drift.
