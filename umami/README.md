# umami

Umami 3 web analytics on PostgreSQL. Picked over Plausible here because it runs in one small container plus Postgres, with no ClickHouse to feed.

```bash
cp .env.example .env
docker compose up -d
cp site.caddy ../proxy/sites/umami.caddy
```

The first login is `admin` / `umami`. Change it immediately.

Add a website in the dashboard, then paste the tracking script it gives you into your site's `<head>`.
