# Home Lab - Docker Learning Environment

A small Docker-based home lab for learning container basics, DNS filtering, metrics collection, and dashboards.

## Quick Start

```bash
# 1. Copy the environment template
cp .env.example .env

# 2. Start the lab
docker compose -f docker/docker-compose.yml up -d
```

Access the services:

- Portainer: `http://localhost:9000`
- Grafana: `http://localhost:3000`
- Prometheus: `http://localhost:9090`
- Pi-hole: `http://localhost/admin`

## What's Included

- Portainer: Docker container management UI
- Pi-hole: DNS-based ad blocking and query visibility
- Prometheus: Metrics collection and storage
- Grafana: Dashboards and visualization

## Credentials

- Grafana: `admin` / `GF_SECURITY_ADMIN_PASSWORD` from `.env`
- Pi-hole: `admin` / `PIHOLE_PASSWORD` from `.env`
- Portainer: set an admin password on first login

## Current State

This repo currently provisions:

- a working Docker Compose stack in `docker/docker-compose.yml`
- a Prometheus datasource in Grafana
- a starter Grafana dashboard at `Home Lab -> Home Lab Overview`

Known limitations:

- Pi-hole v6 no longer exposes the old unauthenticated `/admin/api.php` endpoint, so Pi-hole metrics are not scraped directly by Prometheus in this repo
- the `docker` and `node` Prometheus jobs are placeholders until exporters are added

More detail: [docs/STATUS.md](docs/STATUS.md)

## Useful Commands

```bash
# Start everything
docker compose -f docker/docker-compose.yml up -d

# Check status
docker compose -f docker/docker-compose.yml ps

# View logs
docker compose -f docker/docker-compose.yml logs -f

# Restart one service
docker compose -f docker/docker-compose.yml restart grafana

# Stop everything
docker compose -f docker/docker-compose.yml down
```

## Repository Layout

```text
Home-Lab/
|- docker/
|  \- docker-compose.yml
|- configs/
|  |- prometheus.yml
|  \- grafana/
|- docs/
|  |- SETUP.md
|  |- SERVICES.md
|  |- QUICK_REFERENCE.md
|  |- TROUBLESHOOTING.md
|  \- STATUS.md
|- network/
|  \- ARCHITECTURE.md
|- .env.example
|- .gitignore
\- README.md
```

## Documentation

- [docs/SETUP.md](docs/SETUP.md): setup walkthrough
- [docs/SERVICES.md](docs/SERVICES.md): what each service does
- [docs/QUICK_REFERENCE.md](docs/QUICK_REFERENCE.md): common commands
- [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md): common problems and fixes
- [docs/STATUS.md](docs/STATUS.md): current monitoring and dashboard status
- [network/ARCHITECTURE.md](network/ARCHITECTURE.md): service relationships

## Next Steps

- Explore Portainer to see the containers and volumes
- Open Prometheus and run the `up` query
- Open Grafana and view `Home Lab Overview`
- Add exporters if you want host, Docker, or Pi-hole metrics beyond the starter setup
