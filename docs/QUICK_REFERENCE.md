# Quick Reference

## Start/Stop Commands

```bash
# Start everything
docker compose -f docker/docker-compose.yml up -d

# Stop everything
docker compose -f docker/docker-compose.yml down

# Restart a service
docker compose -f docker/docker-compose.yml restart pihole

# See what's running
docker compose -f docker/docker-compose.yml ps

# View logs (last 50 lines)
docker compose -f docker/docker-compose.yml logs --tail 50

# Follow logs in real-time (Ctrl+C to stop)
docker compose -f docker/docker-compose.yml logs -f

# See specific service logs
docker compose -f docker/docker-compose.yml logs grafana
```

---

## Service URLs

| Service | URL |
|---------|-----|
| **Portainer** | http://localhost:9000 |
| **Grafana** | http://localhost:3000 |
| **Prometheus** | http://localhost:9090 |
| **Pi-hole** | http://localhost/admin |
| **cAdvisor** | http://localhost:8080 |

---

## Default Passwords

| Service | Username | Password |
|---------|----------|----------|
| Grafana | admin | `GF_SECURITY_ADMIN_PASSWORD` from `.env` |
| Pi-hole | admin | `PIHOLE_PASSWORD` from `.env` |
| Portainer | admin | (create on first login) |

---

## Directory Structure

```
Home-Lab/
├── docker/
│   └── docker-compose.yml        # All 4 services
├── configs/
│   └── prometheus.yml            # Prometheus config
├── docs/
│   ├── SETUP.md                  # How to start
│   ├── SERVICES.md               # What each service does
│   ├── TROUBLESHOOTING.md        # Fix problems
│   └── QUICK_REFERENCE.md        # This file
├── network/
│   └── ARCHITECTURE.md           # Network diagrams
├── .env.example                  # Example env variables
├── .gitignore                    # What to ignore in git
└── README.md                     # Project overview
```

---

## Environment Variables (.env)

Copy `.env.example` to `.env` and edit:

```bash
cp .env.example .env
```

Key variables:
- `PIHOLE_PASSWORD` - Change this!
- `GF_SECURITY_ADMIN_PASSWORD` - Change this!
- `TZ` - Your timezone

---

## Common Commands

### Docker Basics
```bash
# See resource usage
docker stats

# List all containers
docker ps -a

# List images
docker images

# Remove old data (free up space)
docker system prune

# Get into a container shell
docker exec -it pihole /bin/bash
```

### Logs & Debugging
```bash
# See why something failed
docker compose -f docker/docker-compose.yml logs pihole

# Follow logs (real-time)
docker compose -f docker/docker-compose.yml logs -f grafana

# See last 20 lines only
docker compose -f docker/docker-compose.yml logs --tail 20 prometheus
```

### Data Management
```bash
# See volumes
docker volume ls

# Remove unused volumes
docker volume prune

# See networks
docker network ls
```

---

## File Locations

On your computer:
- Config files: `Home-Lab/configs/`
- Documentation: `Home-Lab/docs/`
- Docker setup: `Home-Lab/docker/docker-compose.yml`
- Network info: `Home-Lab/network/ARCHITECTURE.md`

---

## Learning Path

1. **Get comfortable with Docker**
   - Start all services: `docker compose -f docker/docker-compose.yml up -d`
   - View containers: `docker compose -f docker/docker-compose.yml ps`
   - Check resource usage: `docker stats`

2. **Explore each service**
   - Portainer: http://localhost:9000
   - Pi-hole: http://localhost/admin
   - Prometheus: http://localhost:9090
   - Grafana: http://localhost:3000
   - cAdvisor: http://localhost:8080

3. **Try basic tasks**
   - Change passwords
   - View Pi-hole stats
   - Create a Prometheus query
   - Check host metrics with `node_uname_info`
   - Check container metrics with `container_memory_usage_bytes`
   - Open the `Home Lab Overview` Grafana dashboard

4. **Experiment**
   - Stop/restart services
   - View logs
   - Change configurations
   - Try adding a new container

---

## Quick Troubleshooting

```bash
# Something broken? Try this first:
docker compose -f docker/docker-compose.yml down
docker compose -f docker/docker-compose.yml up -d
docker compose -f docker/docker-compose.yml ps

# Still broken? Check logs:
docker compose -f docker/docker-compose.yml logs

# See what's using CPU/Memory:
docker stats

# Free up space:
docker system prune
```

---

## Files to Check

- **Problems?** → Check `TROUBLESHOOTING.md`
- **Setup help?** → Check `SETUP.md`
- **Service questions?** → Check `SERVICES.md`
- **Network questions?** → Check `network/ARCHITECTURE.md`

---

## Useful Links

- [Docker Docs](https://docs.docker.com/)
- [Docker Compose Docs](https://docs.docker.com/compose/)
- [Grafana Docs](https://grafana.com/docs/)
- [Prometheus Docs](https://prometheus.io/docs/)
- [Pi-hole Docs](https://docs.pi-hole.net/)


