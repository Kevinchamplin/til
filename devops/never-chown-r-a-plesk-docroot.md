# Never `chown -R` a Plesk docroot

Tempting after a deploy ("just make everything owned by my user") — and it will hand you a **403 Forbidden** on the whole site.

Plesk serves files through a model where the document root and its contents must be owned by the subscription's system user **and** carry the right group (commonly `psaserv` / `psacln`) with specific perms. A blanket `chown -R deployuser:deployuser httpdocs` strips that group, and the web server (running as its own user) can no longer read the tree → 403.

## Fix / habit

- Only `chown` the **files you actually deployed**, to the subscription's user, and leave the group alone.
- If you've already nuked it, restore the expected ownership/group rather than guessing — e.g. files back to `subuser:psaserv`, dirs `755`, files `644`.
- Different vhosts run as different system users; don't assume one deploy user across sites.

## Takeaway

On panel-managed hosting, ownership/group is load-bearing for the web server's read access. Scope `chown` to deployed files; never `-R` the docroot.
