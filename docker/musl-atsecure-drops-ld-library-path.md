# musl drops `LD_LIBRARY_PATH` when `ruid != euid`

Alpine/musl-based containers: an app that runs with **real uid 0 but effective uid N** (the common privilege-drop pattern) spawns child processes flagged `AT_SECURE=1`. Under `AT_SECURE`, the musl dynamic loader **ignores `LD_LIBRARY_PATH` and `$ORIGIN`** as a security measure.

Result: a helper binary (LibreOffice/`soffice`, headless Chromium, a bundled toolchain) can't find its `.so` files — **but only when launched from the app**. It works fine under `docker exec`, because that shell runs ruid == euid, so the loader honors `LD_LIBRARY_PATH`. That mismatch makes it maddening to debug.

## Fix

Add the library directory to musl's **trusted** search path so it's found regardless of `AT_SECURE`:

```dockerfile
RUN echo "/opt/myapp/lib" >> /etc/ld-musl-x86_64.path
```

Append it (last), so you don't shadow system libs.

## Takeaway

"Works in `docker exec`, fails from the app" + Alpine + a priv-drop process = the `AT_SECURE` loader rule. Don't rely on `LD_LIBRARY_PATH`; register the path in `/etc/ld-musl-*.path`.
