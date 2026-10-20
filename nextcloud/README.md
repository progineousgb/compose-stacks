# nextcloud

Nextcloud 34 (Apache image), MariaDB, Redis for file locking and caching, and a separate container that runs `cron.php` every 5 minutes.

```bash
cp .env.example .env
docker compose up -d
cp site.caddy ../proxy/sites/nextcloud.caddy
```

The admin account is created on first start from `ADMIN_USER` / `ADMIN_PASSWORD`. Those variables are only read once; changing them later does nothing.

After the first login, set background jobs to "Cron" under Administration settings > Basic settings, since the `cron` container is already running it.

Plan for RAM. Nextcloud with MariaDB and Redis is comfortable on 2 GB, tight on 1 GB.

`occ` commands:

```bash
docker compose exec -u www-data nextcloud php occ status
```
