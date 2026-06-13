# Tailwind v4: `.hidden` loses to `.flex`/`.grid`/`.inline-flex`

In Tailwind v4, utilities no longer carry `!important`, so conflicts are resolved by **source order** in the generated stylesheet. The display utilities (`.flex`, `.grid`, `.inline-flex`) are emitted *after* `.hidden`. So on an element that has both:

```html
<div class="hidden md:flex">…</div>
```

…the `.inline-flex`/`.flex` rule wins the cascade at *all* widths, and your "hidden by default" element is actually visible. Responsive variants that toggle display can behave the opposite of what you intend.

## Fix

Use the **native HTML `hidden` attribute** when you need a hard default-hidden, because the UA stylesheet's `[hidden] { display: none }` is reliable and you control it independently of utility order:

```html
<div hidden class="md:flex">…</div>
```

Or scope visibility so the same element never carries two competing display utilities.

## Takeaway

v4 dropped `!important` from utilities — equal-specificity conflicts are decided by emit order, and `display` beats `.hidden`. Reach for the native `hidden` attribute for guaranteed default-hidden.
