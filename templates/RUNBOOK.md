# Runbook

## Start

```bash
docker compose up -d --build
```

## Healthcheck

```bash
curl -f http://localhost:3001/health
```

## Logs

```bash
docker compose logs --tail=200 api
```

## Backup

```bash
./scripts/backup.sh
```

## Restore

```bash
./scripts/restore.sh ./backups/latest.db
```

## Update

```bash
git pull
docker compose up -d --build
```

## Rollback

Document the exact rollback process for this app.

## Human approval required

Do not automate without explicit user approval:

- destructive data changes
- database migrations
- credential changes
- production deployment changes
- external notifications or purchases
