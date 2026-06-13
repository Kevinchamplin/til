# SSH heredocs mangle `$` and can corrupt PHP

Writing a file on a remote host through an **unquoted** heredoc lets your *local* shell expand everything first:

```bash
ssh host <<EOF
cat > /var/www/app/Foo.php <<'INNER'
<?php
\$value = $undefined_locally;   # local shell ate $undefined_locally → ""
INNER
EOF
```

Every `$var`, backtick, and `\` is interpreted locally before it's sent. For PHP that means variables get blanked or mangled — and the file becomes syntactically broken in subtle ways. Worse, frameworks with class auto-discovery (Laravel) will **silently skip** the unparseable class with no boot error; the only symptom is "command not defined" or a route that 404s.

## Fix

- **Quote the heredoc delimiter** (`<<'EOF'`) so nothing local-expands, or
- Don't pipe code over SSH at all — `scp`/`rsync` the file, or commit + pull on the remote.
- After any remote write, lint the tree: `find . -name '*.php' -print0 | xargs -0 -n1 php -l`.

## Takeaway

Unquoted heredocs run your local shell over the payload. Quote the delimiter, or transfer files as files. Always `php -l` after a remote write — auto-discovery hides the breakage.
