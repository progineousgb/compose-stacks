# wordpress

WordPress (Apache image) with MariaDB 11.8 LTS. The database is only reachable on the stack's own network.

```bash
cp .env.example .env
docker compose up -d
cp site.caddy ../proxy/sites/wordpress.caddy   # edit the hostname
```

`WORDPRESS_CONFIG_EXTRA` tells WordPress it is behind HTTPS so you do not get redirect loops in wp-admin.

Backups: dump the database with

```bash
docker compose exec db sh -c 'mariadb-dump -u root -p"$MARIADB_ROOT_PASSWORD" --single-transaction "$MARIADB_DATABASE"' > wp.sql
```

and copy the `wp_html` volume (uploads, themes, plugins).

Upload size defaults to the PHP image setting (2 MB). To raise it, mount a file into `/usr/local/etc/php/conf.d/uploads.ini` with `upload_max_filesize` and `post_max_size`.
