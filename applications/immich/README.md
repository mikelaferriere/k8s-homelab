https://github.com/immich-app/immich-charts

# Restore from backup
```bash
gunzip < "/home/mike/Downloads/immich-db-backup-1734746400058.sql.gz" | sed "s/SELECT pg_catalog.set_config('search_path', '', false);/SELECT pg_catalog.set_config('search_path', 'public, pg_catalog', true);/g" | kubectl -n immich exec -it immich-postgresql-0 -- psql U immich -d immich
```