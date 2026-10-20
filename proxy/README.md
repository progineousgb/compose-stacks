# proxy

Caddy in front of every other stack. It owns ports 80 and 443, gets Let's Encrypt certificates on its own and creates the shared `proxy` network the apps join.

```bash
cp .env.example .env        # set ACME_EMAIL
docker compose up -d
```

Each app folder has a `site.caddy`. Copy it into `sites/`, change the hostname, then reload:

```bash
cp ../vaultwarden/site.caddy sites/vaultwarden.caddy
docker compose exec -w /etc/caddy caddy caddy reload
```

The DNS A/AAAA record for the hostname has to point at the server before Caddy can get a certificate. Certificates live in the `caddy_data` volume; do not delete it or you will hit Let's Encrypt rate limits on re-issue.
