# n8n

n8n with PostgreSQL instead of the default SQLite, which holds up better once you have a lot of executions.

```bash
cp .env.example .env
docker compose up -d
cp site.caddy ../proxy/sites/n8n.caddy
```

Open `https://<N8N_HOST>` and create the owner account straight away; until you do, anyone who finds the URL can.

`N8N_ENCRYPTION_KEY` encrypts stored credentials. If you lose it, every credential has to be re-entered. Back it up with the database.

Execution data grows quickly. Look at `EXECUTIONS_DATA_PRUNE` and `EXECUTIONS_DATA_MAX_AGE` in the n8n docs if the disk starts filling up.
