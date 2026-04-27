# Quick Start Guide

## What You'll Learn

This lab teaches you about:

- **Docker**: running applications in containers
- **DNS**: how domain names work with Pi-hole
- **Metrics**: collecting system health data with Prometheus
- **Dashboards**: visualizing data with Grafana
- **Exporters**: sending host and container metrics into Prometheus

## Prerequisites

- Docker installed ([Download here](https://www.docker.com/products/docker-desktop))
- Docker Compose, included with Docker Desktop
- Around 4 GB RAM available
- Terminal or PowerShell access

## Installation

### Step 1: Open The Project

```bash
cd Home-Lab/
```

### Step 2: Copy Environment File

```bash
cp .env.example .env
```

### Step 3: Start Docker Services

```bash
docker compose -f docker/docker-compose.yml up -d
```

That's it. Your services are running.

## Access Your Services

Open these URLs in your browser:

| Service | URL | Login |
|---------|-----|-------|
| **Portainer** (manage containers) | http://localhost:9000 | Set on first login |
| **Pi-hole** (block ads) | http://localhost/admin | Password: `PIHOLE_PASSWORD` from `.env` |
| **Prometheus** (see metrics) | http://localhost:9090 | No login needed |
| **Grafana** (make charts) | http://localhost:3000 | admin / `GF_SECURITY_ADMIN_PASSWORD` from `.env` |
| **cAdvisor** (container metrics) | http://localhost:8080 | No login needed |

Node Exporter is scraped internally by Prometheus at `node-exporter:9100`, so it does not expose a browser UI by default.

## First-Time Setup

### Change Passwords

These are default passwords. Change them after the first run.

**Pi-hole**

1. Go to http://localhost/admin
2. Open Settings
3. Change the password

**Grafana**

1. Go to http://localhost:3000
2. Open your profile
3. Change the password

## Understanding The Services

### Portainer

- **What**: Container management dashboard
- **Why**: Visual way to see and manage your containers
- **Learn**: Container lifecycle, images, volumes

### Pi-hole

- **What**: DNS-level ad blocker
- **Why**: Blocks ads network-wide and improves query visibility
- **Learn**: DNS, blocklists, allowlists, local network filtering

### Prometheus

- **What**: Metrics database
- **Why**: Collects time-series data from services and exporters
- **Learn**: Scrape targets, PromQL, monitoring basics

### Grafana

- **What**: Dashboard and visualization tool
- **Why**: Turns Prometheus data into charts and panels
- **Learn**: Dashboards, panels, queries, alerts

### Node Exporter

- **What**: Host metrics exporter
- **Why**: Gives Prometheus CPU, memory, disk, filesystem, and network data
- **Learn**: Host monitoring

### cAdvisor

- **What**: Container metrics exporter and UI
- **Why**: Gives Prometheus per-container CPU, memory, filesystem, and network data
- **Learn**: Container monitoring

## First Metrics To Try

Open Prometheus at http://localhost:9090 and try:

```promql
up
```

```promql
node_uname_info
```

```promql
container_memory_usage_bytes
```

Then open Grafana at http://localhost:3000 and view `Dashboards -> Home Lab -> Home Lab Overview`.

## Common Commands

```bash
# See what's running
docker compose -f docker/docker-compose.yml ps

# View logs for a service
docker compose -f docker/docker-compose.yml logs pihole
docker compose -f docker/docker-compose.yml logs grafana
docker compose -f docker/docker-compose.yml logs prometheus

# Restart a service
docker compose -f docker/docker-compose.yml restart pihole

# Stop everything
docker compose -f docker/docker-compose.yml down

# Start again
docker compose -f docker/docker-compose.yml up -d

# See resource usage
docker stats
```

## Troubleshooting

### Can't Access Services?

```bash
# Check if containers are running
docker compose -f docker/docker-compose.yml ps

# If not running, restart
docker compose -f docker/docker-compose.yml down
docker compose -f docker/docker-compose.yml up -d
```

### Prometheus Target Missing?

```bash
# Check Prometheus logs
docker compose -f docker/docker-compose.yml logs prometheus

# Check exporter logs
docker compose -f docker/docker-compose.yml logs node-exporter
docker compose -f docker/docker-compose.yml logs cadvisor
```

### Container Crashed?

```bash
# See what happened
docker compose -f docker/docker-compose.yml logs pihole

# Restart it
docker compose -f docker/docker-compose.yml restart pihole
```

## Next Steps

Once comfortable with these services, you can:

- Create richer Grafana dashboards from Node Exporter and cAdvisor metrics
- Set up Pi-hole filtering rules
- Add a Pi-hole v6 metrics exporter
- Explore Docker networking
- Build your own container

## Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Guide](https://docs.docker.com/compose/)
- [Pi-hole Docs](https://docs.pi-hole.net/)
- [Grafana Docs](https://grafana.com/docs/grafana/latest/)
- [Prometheus Docs](https://prometheus.io/docs/)
