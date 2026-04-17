---
name: docker-restart
description: Restart Docker services smartly. Use when the user changes nginx.conf, docker-compose.yml, Dockerfile, backend dependencies, or wants to restart/rebuild a specific service. Knows which service to restart vs full rebuild.
---

The user wants to restart or rebuild Docker services for the sidekick-app project.

## Project Docker Setup
- **backend**: Node.js on :8080, hot-reload via volume mount `./backend:/app`
- **frontend**: Expo on :8081, hot-reload via volume mount `.:/app`
- **nginx**: Reverse proxy on :80, config mounted read-only from `nginx.conf`
- Health checks: backend checks `/health`, frontend checks `/`, nginx depends on both

## Decision Rules

**Full rebuild required** (use `docker compose up --build -d <service>`):
- `Dockerfile` or `Dockerfile.frontend` changed
- `package.json` or `package-lock.json` changed (new deps)
- User says "rebuild" or "fresh build"

**Soft restart sufficient** (use `docker compose restart <service>`):
- `nginx.conf` changed → restart nginx only
- `docker-compose.yml` changed → `docker compose up -d` (recreates changed services)
- Backend source code changed → usually hot-reloads automatically, but restart backend if stuck
- User says "restart" without specifying

**Full stack restart** (use `docker compose down && docker compose up -d`):
- Multiple services affected
- Something is broken and unclear which service

## Steps to Follow

1. Ask the user what changed (if not already stated), or infer from context.
2. Pick the appropriate command from the decision rules above.
3. Run the command in the terminal from `c:/Users/liorm/sidekick-app`.
4. After restart, check health with: `docker compose ps` and show status.
5. If any service is unhealthy, tail its logs: `docker compose logs --tail=30 <service>`.
6. Report: which services restarted, their health status, and any errors found.

## Common Commands Reference
```bash
# Soft restart a service
docker compose restart nginx

# Recreate services after docker-compose.yml change
docker compose up -d

# Rebuild a specific service (new deps or Dockerfile change)
docker compose up --build -d backend

# Full teardown and restart
docker compose down && docker compose up -d

# Check health status
docker compose ps

# Tail logs
docker compose logs --tail=30 backend
docker compose logs --tail=30 frontend
docker compose logs --tail=30 nginx
```
