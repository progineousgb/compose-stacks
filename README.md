<p align="center"><img src="assets/logo.png" alt="Pro Gineous" width="360"></p>

# compose-stacks

Docker Compose files for running a handful of common self-hosted apps on one VPS behind a single Caddy reverse proxy. Caddy handles HTTPS certificates; each app is its own Compose project so you can start, stop and upgrade them independently.

```
proxy/          Caddy, ports 80/443, creates the "proxy" network
wordpress/      WordPress + MariaDB
n8n/            n8n + PostgreSQL
uptime-kuma/    Uptime Kuma
nextcloud/      Nextcloud + MariaDB + Redis + cron
umami/          Umami analytics + PostgreSQL
vaultwarden/    Vaultwarden (Bitwarden-compatible)
```

## How it fits together

`proxy` creates a Docker network called `proxy`. App containers that serve HTTP join that network; databases stay on the app's private default network and never publish a port. Caddy reaches each app by service name, so no app port is exposed on the host.

## Setup

You need Docker Engine with the Compose plugin, and DNS records for each hostname pointing at the server.

```bash
git clone https://gitlab.com/pro-gineous-group/compose-stacks.git
cd compose-stacks/proxy
cp .env.example .env && $EDITOR .env
docker compose up -d

cd ../vaultwarden
cp .env.example .env && $EDITOR .env
docker compose up -d
cp site.caddy ../proxy/sites/vaultwarden.caddy && $EDITOR ../proxy/sites/vaultwarden.caddy
docker compose -f ../proxy/compose.yaml exec -w /etc/caddy caddy caddy reload
```

Repeat for the other apps. Generate passwords with `openssl rand -hex 24`.

Open only 22, 80 and 443 in the firewall. Note that Docker writes its own iptables rules, so a port published with `ports:` bypasses UFW. That is why only Caddy publishes anything here.

## Upgrading

Image tags are pinned. To upgrade, change the tag in `compose.yaml`, read the app's release notes, then:

```bash
docker compose pull && docker compose up -d
```

Major database upgrades (PostgreSQL 17 to 18, for example) need a dump and restore, not just a new tag.

## Backups

Named volumes live under `/var/lib/docker/volumes`. Stop the stack or dump the database first, then copy the volume. Each app README has the specifics.

## Validation

Every stack is checked with `docker compose config` using its `.env.example`:

```bash
for d in */; do (cd "$d" && cp .env.example .env && docker compose config -q && echo "ok $d"; rm .env); done
```

## License

MIT, see [LICENSE](LICENSE).

---

Maintained by [Pro Gineous](https://progineous.com/). More scripts for VPS work are in the [server toolkit](https://gitlab.com/pro-gineous-group/server-toolkit).
