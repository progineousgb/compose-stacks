# uptime-kuma

Uptime Kuma 2.x with its built-in SQLite storage. Configuration happens in the browser.

```bash
docker compose up -d
cp site.caddy ../proxy/sites/uptime-kuma.caddy
```

Create the admin user on first visit. Websockets work through Caddy without extra config.

Monitoring from the same server you are monitoring only tells you the app is up, not that the server is reachable from outside. For that, run a second copy somewhere else.
