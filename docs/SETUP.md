# Quick Start Guide

## What You'll Learn

This lab teaches you about:
- **Docker**: Running applications in containers
- **DNS**: How domain names work (Pi-hole)
- **Metrics**: Monitoring system health (Prometheus)
- **Dashboards**: Visualizing data (Grafana)

## Prerequisites

- Docker installed ([Download here](https://www.docker.com/products/docker-desktop))
- Docker Compose (included with Docker Desktop)
- ~4GB RAM available
- Terminal/PowerShell access

## Installation (5 minutes)

### Step 1: Copy Environment File
```bash
cd Home-Lab/
cp .env.example .env
```

### Step 2: Start Docker Services
```bash
docker compose -f docker/docker-compose.yml up -d
```

That's it! Your services are running.

## Access Your Services

Open these URLs in your browser:

| Service | URL | Login |
|---------|-----|-------|
| **Portainer** (manage containers) | http://localhost:9000 | Set on first login |
| **Pi-hole** (block ads) | http://localhost/admin | Password: `PIHOLE_PASSWORD` from `.env` |
| **Prometheus** (see metrics) | http://localhost:9090 | No login needed |
| **Grafana** (make charts) | http://localhost:3000 | admin / `GF_SECURITY_ADMIN_PASSWORD` from `.env` |

## First-Time Setup

### Change Passwords!
These are default passwords - change them:

**Pi-hole**:
1. Go to http://localhost/admin
2. Settings → Change Password

**Grafana**:
1. Go to http://localhost:3000
2. Click profile icon → Change password

## Understanding the Services

### 🐳 Portainer
- **What**: Container management dashboard
- **Why**: Visual way to see and manage your containers
- **Learn**: Container lifecycle, images, volumes

### 🔒 Pi-hole
- **What**: DNS-level ad blocker
- **Why**: Blocks ads network-wide, protects privacy
- **Learn**: How DNS works, ad blocking

### 📊 Prometheus
- **What**: Metrics database
- **Why**: Collects data about how services are performing
- **Learn**: Time-series data, monitoring

### 📈 Grafana
- **What**: Dashboard and visualization tool
- **Why**: Makes Prometheus data pretty and useful
- **Learn**: Data visualization, creating alerts

## Common Commands

```bash
# See what's running
docker compose -f docker/docker-compose.yml ps

# View logs for a service
docker compose -f docker/docker-compose.yml logs pihole        # see Pi-hole logs
docker compose -f docker/docker-compose.yml logs grafana       # see Grafana logs

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

### Can't access services?
```bash
# Check if containers are running
docker compose -f docker/docker-compose.yml ps

# If not running, restart
docker compose -f docker/docker-compose.yml down
docker compose -f docker/docker-compose.yml up -d
```

### Forgot password?
```bash
# Restart Grafana (resets to admin/admin123)
docker compose -f docker/docker-compose.yml restart grafana

# For Pi-hole, check .env file for password
Get-Content .env
```

### Container crashed?
```bash
# See what happened
docker compose -f docker/docker-compose.yml logs pihole

# Restart it
docker compose -f docker/docker-compose.yml restart pihole
```

## Next Steps

Once comfortable with these 4 services, you can:
- Add more containers (try adding a web server)
- Create Grafana dashboards
- Set up Pi-hole filtering rules
- Explore Docker networking
- Build your own container

## Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Guide](https://docs.docker.com/compose/)
- [Pi-hole Docs](https://docs.pi-hole.net/)
- [Grafana Docs](https://grafana.com/docs/grafana/latest/)


