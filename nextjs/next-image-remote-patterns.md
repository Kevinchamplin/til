# `next/image` `remotePatterns` for a headless CMS

`next/image` refuses to optimize images from a host you haven't allow-listed, failing with:

```
Error: Invalid src prop ... hostname "cms.example.com" is not configured under images in your next.config.js
```

Allow the CMS host explicitly in `next.config.js`:

```js
module.exports = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'cms.example.com',
        pathname: '/wp-content/uploads/**',
      },
    ],
  },
};
```

Notes from doing this in production:

- Prefer `remotePatterns` over the legacy `domains` array — it lets you pin `protocol` and constrain `pathname`, so you're not optimizing arbitrary paths on that host.
- `hostname` does **not** accept a full URL or a port baked into the string — host only.
- Restart `next dev`; this config isn't hot-reloaded.

## Takeaway

Allow-list the CMS image host with `remotePatterns`, scope it to the uploads path, and remember it needs a dev-server restart to take effect.
