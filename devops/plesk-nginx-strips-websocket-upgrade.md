# Plesk nginx strips the WebSocket `Upgrade` header

Reverse-proxying a WebSocket app behind Plesk's nginx, the handshake keeps failing (HTTP 200 instead of `101 Switching Protocols`, or connections that drop immediately). The proxy at the default `location /` doesn't forward the `Upgrade`/`Connection` headers, so the upgrade never happens.

Two things fixed it for me:

1. **Serve the socket on its own path** (e.g. `/ws`) rather than `/`, so you can add a dedicated proxy block just for it.
2. Add that block via Plesk's **Additional nginx directives** (`vhost_nginx.conf`), explicitly passing the upgrade headers:

```nginx
location /ws {
    proxy_pass http://127.0.0.1:4790;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
    proxy_read_timeout 3600s;
}
```

Plesk's Apache layer has the same class of issue; sometimes proxying through Apache is the cleaner path when nginx fights you.

## Takeaway

WebSockets need `proxy_http_version 1.1` + the `Upgrade`/`Connection` headers. On Plesk, give the socket a dedicated path and set them in `vhost_nginx.conf` — the default `location /` won't.
