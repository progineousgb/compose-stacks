# vaultwarden

Vaultwarden, the Bitwarden-compatible server written in Rust. Works with the official Bitwarden apps and browser extensions.

```bash
cp .env.example .env
docker compose up -d
cp site.caddy ../proxy/sites/vaultwarden.caddy
```

Signups are off by default. To create your own account, set `SIGNUPS_ALLOWED=true`, run `docker compose up -d`, register, then flip it back and run `docker compose up -d` again.

The Bitwarden clients refuse plain HTTP, so the stack only works once Caddy has a certificate for `VW_HOST`.

Everything lives in the `vw_data` volume (SQLite database, attachments, RSA keys). Back it up; a password manager you cannot restore is worse than none.
