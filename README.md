# Today I Learned

A running collection of short, focused notes on things I learned (often the hard way) shipping production software — headless WordPress, Laravel, Next.js, Docker/Alpine, CSS, and the DevOps glue around them.

Each note is small on purpose: the problem, *why* it happens, and the fix. The kind of thing that costs an hour the first time and thirty seconds every time after, once it's written down.

> Inspired by the [TIL convention](https://github.com/jbranchaud/til) — capture the small wins so they compound.

## Categories

### git
- [Use git worktrees for parallel agents/sessions](git/use-worktrees-for-parallel-agents.md)
- [How `Co-authored-by:` trailers actually work](git/co-authored-by-trailer.md)

### WordPress
- [WPGraphQL returns ACF `select` fields as an array](wordpress/wpgraphql-acf-select-returns-array.md)
- [WP-CLI needs a PHP binary with `mysqli`](wordpress/wp-cli-needs-php-with-mysqli.md)

### Laravel
- [Spatie multitenancy: queue tenant-awareness default](laravel/spatie-multitenancy-queue-default.md)
- [Blade: `@` directly before `{{ }}` breaks interpolation](laravel/blade-at-before-echo.md)

### MySQL
- [Status `ENUM` truncation fails silently in the producer](mysql/status-enum-truncation-is-silent.md)

### Docker / Alpine
- [musl drops `LD_LIBRARY_PATH` when `ruid != euid`](docker/musl-atsecure-drops-ld-library-path.md)

### CSS
- [Tailwind v4: `.hidden` loses to `.flex`/`.grid`/`.inline-flex`](css/tailwind-v4-hidden-vs-display.md)
- [`position: sticky` breaks under an `overflow` ancestor](css/sticky-breaks-under-overflow-clip.md)

### Next.js
- [Tag-based ISR revalidation from a CMS webhook](nextjs/isr-tag-based-revalidation.md)
- [`next/image` `remotePatterns` for a headless CMS](nextjs/next-image-remote-patterns.md)

### DevOps
- [Never `chown -R` a Plesk docroot](devops/never-chown-r-a-plesk-docroot.md)
- [Plesk nginx strips the WebSocket `Upgrade` header](devops/plesk-nginx-strips-websocket-upgrade.md)

### Observability
- [New Relic auto-RUM injects inline scripts and breaks strict CSP](observability/newrelic-autorum-breaks-strict-csp.md)

### AI / LLMs
- [`gemini-2.0-flash` 404s on `generateContent`](ai/gemini-2-0-flash-retired-on-generatecontent.md)

### Shell
- [SSH heredocs mangle `$` and can corrupt PHP](shell/ssh-heredoc-dollar-escaping-corrupts-php.md)

---

License: [MIT](LICENSE)
