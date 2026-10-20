# Contributing

- One app per folder: `compose.yaml`, `.env.example`, `site.caddy`, `README.md`.
- Pin image tags to a real release. No `latest`.
- Only the proxy publishes ports. Databases stay on the stack's default network.
- No real secrets, domains or IPs in examples.
- Run `docker compose config -q` with the `.env.example` copied to `.env` before opening a merge request.
