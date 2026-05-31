# Home Lab - Docker Learning Environment

A Docker-based home lab for learning container management, DNS filtering, metrics collection, uptime monitoring, and reverse proxying.

## Quick Start

```bash
# 1. Copy the environment template
cp .env.example .env

# 2. Edit .env and set your passwords
nano .env

# 3. Start the stack
docker compose -f docker/docker-compose.yml up -d
```

## Services

| Service | Purpose | Access |
|---|---|---|
| Portainer | Container management UI | http://localhost:9000 |
| Pi-hole | DNS-based ad blocking | http://localhost:8081/admin |
| Prometheus | Metrics collection | http://localhost:9090 |
| Grafana | Dashboards and visualization | http://localhost:3000 |
| Uptime Kuma | Service uptime monitoring | http://localhost:3001 |
| Nginx Proxy Manager | Reverse proxy with SSL | http://localhost:81 (admin) |
| cAdvisor | Per-container metrics | http://localhost:8080 |
| Node Exporter | Host metrics (scraped by Prometheus) | — |
| Pi-hole Exporter | Pi-hole metrics for Prometheus | — |

## Credentials

| Service | Username | Password |
|---|---|---|
| Grafana | `admin` | `GF_SECURITY_ADMIN_PASSWORD` from `.env` |
| Pi-hole | `admin` | `PIHOLE_PASSWORD` from `.env` |
| Nginx Proxy Manager | `admin@example.com` | `changeme` (change on first login) |
| Portainer | set on first login | — |

## Grafana Dashboards

Open Grafana at http://localhost:3000 and navigate to **Home Lab → Home Lab Overview**.

Panels included:
- CPU, memory, and disk gauges
- System uptime and load average
- Services up count
- CPU and memory time series
- Network I/O and disk I/O
- Per-container CPU and memory
- Container status table
- Pi-hole: queries today, ads blocked, block rate, blocklist size

## Useful Commands

```bash
# Start everything
docker compose -f docker/docker-compose.yml up -d

# Check status
docker compose -f docker/docker-compose.yml ps

# View all logs
docker compose -f docker/docker-compose.yml logs -f

# View logs for one service
docker compose -f docker/docker-compose.yml logs -f grafana

# Restart one service
docker compose -f docker/docker-compose.yml restart grafana

# Pull latest images and recreate
docker compose -f docker/docker-compose.yml pull
docker compose -f docker/docker-compose.yml up -d

# Stop everything
docker compose -f docker/docker-compose.yml down

# Stop and remove volumes (full reset)
docker compose -f docker/docker-compose.yml down -v
```

## Repository Layout

```
Home-Lab/
├── docker/
│   └── docker-compose.yml
├── configs/
│   ├── prometheus.yml
│   └── grafana/
│       └── provisioning/
│           ├── datasources/
│           └── dashboards/
│               └── json/
├── docs/
│   ├── SETUP.md
│   ├── SERVICES.md
│   ├── QUICK_REFERENCE.md
│   ├── TROUBLESHOOTING.md
│   └── STATUS.md
├── network/
│   └── ARCHITECTURE.md
├── .env.example
└── README.md
```

## Documentation

- [docs/SETUP.md](docs/SETUP.md) — setup walkthrough
- [docs/SERVICES.md](docs/SERVICES.md) — what each service does
- [docs/QUICK_REFERENCE.md](docs/QUICK_REFERENCE.md) — common commands
- [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) — common problems and fixes
- [docs/STATUS.md](docs/STATUS.md) — current monitoring status
- [network/ARCHITECTURE.md](network/ARCHITECTURE.md) — service relationships
