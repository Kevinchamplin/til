# WP-CLI needs a PHP binary with `mysqli`

On managed hosting (Plesk, cPanel) the **system** `php` on the `PATH` is often a minimal CLI build without the `mysqli`/`mysqlnd` extension. Run WP-CLI with it and every command that touches the database dies with:

```
Error: There has been a critical error... call to undefined function mysqli_...
```

The web stack uses a *different*, fully-loaded PHP managed by the panel. Point WP-CLI at that one:

```bash
# Plesk ships per-version PHP under /opt/plesk/php/<ver>/bin/php
/opt/plesk/php/8.3/bin/php /usr/local/bin/wp core version --path=/path/to/wordpress
```

Check what a binary actually has before blaming WordPress:

```bash
php -m | grep -i mysqli        # the one on PATH
/opt/plesk/php/8.3/bin/php -m | grep -i mysqli   # the panel's
```

## Takeaway

If WP-CLI can't reach the DB, it's usually the *PHP binary*, not WordPress. Invoke `wp` with the panel-managed PHP that has `mysqli` compiled in, and always pass an explicit `--path`.
