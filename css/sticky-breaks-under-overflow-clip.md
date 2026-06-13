# `position: sticky` breaks under an `overflow` ancestor

A `position: sticky` element silently stops sticking if **any ancestor** has `overflow: hidden`, `overflow: auto`, or `overflow: scroll` on the relevant axis. No error, no warning — it just scrolls away like `static`.

The reason: a `sticky` element is positioned relative to its nearest *scroll container*. An ancestor with `overflow: hidden` becomes that scroll container, so "stick to the viewport" turns into "stick within this clipped box," which usually means it never sticks at all.

The classic trigger is a global `overflow-x: hidden` (or Tailwind `overflow-x-hidden`) added to `body`/a wrapper to kill horizontal scroll — it quietly kills sticky headers three levels down.

## Fix

Use `overflow-x: clip` instead of `hidden` where you only need to prevent horizontal overflow — `clip` doesn't create a scroll container, so descendant `sticky` keeps working:

```css
.wrapper { overflow-x: clip; } /* Tailwind: overflow-x-clip */
```

## Takeaway

When sticky "just stops," walk **every** ancestor for an `overflow` value. Prefer `clip` over `hidden` for overflow guards so you don't break sticky descendants.
