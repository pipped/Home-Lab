# Troubleshooting Guide

## Quick Checks

Before diving deeper, always try:

```bash
# See what's running
docker compose -f docker/docker-compose.yml ps

# Restart everything
docker compose -f docker/docker-compose.yml down
docker compose -f docker/docker-compose.yml up -d

# Check logs
docker compose -f docker/docker-compose.yml logs
```

---

## Services Won't Start

**Symptom**: `docker compose -f docker/docker-compose.yml up -d` fails or containers crash

**Solution**:
```bash
# See what went wrong
docker compose -f docker/docker-compose.yml logs

# See specific service
docker compose -f docker/docker-compose.yml logs pihole
docker compose -f docker/docker-compose.yml logs grafana

# Restart Docker (nuclear option)
docker restart

# Or on Linux:
sudo systemctl restart docker
```

---

## Can't Access Services

**Symptom**: "Connection refused" or "Cannot reach localhost:9000"

**Solution**:
```bash
# Check if containers are actually running
docker compose -f docker/docker-compose.yml ps

# See if ports are listening
netstat -an | findstr :9000      # Windows PowerShell
ss -tlnp | grep :9000            # Linux

# Try restarting the service
docker compose -f docker/docker-compose.yml restart portainer
```

---

## Pi-hole Not Blocking Ads

**Symptom**: Ads still showing up

**Common Reasons**:
1. Not using Pi-hole as DNS server
2. Adlists not enabled
3. Service crashed

**Fix**:
```bash
# Check logs
docker compose -f docker/docker-compose.yml logs pihole

# Restart it
docker compose -f docker/docker-compose.yml restart pihole

# Check it's running
docker compose -f docker/docker-compose.yml ps pihole

# In Pi-hole admin panel:
# Settings → Adlists → Add some blocklists → Save
```

---

## Forgot Passwords

**Grafana**:
```bash
# Check the configured password in .env
Get-Content .env | Select-String GF_SECURITY_ADMIN_PASSWORD
```

**Pi-hole**:
```bash
# Check your .env file
Get-Content .env | Select-String PIHOLE_PASSWORD

# Or view the docker-compose.yml
Get-Content docker/docker-compose.yml | Select-String FTLCONF_webserver_api_password
```

---

## Prometheus Not Collecting Data

**Symptom**: Empty graphs, no data in Prometheus

**Solution**:
```bash
# Check logs
docker compose -f docker/docker-compose.yml logs prometheus

# Verify config file exists
ls configs/prometheus.yml

# Restart Prometheus
docker compose -f docker/docker-compose.yml restart prometheus
```

**Important**:

- Pi-hole v6 does not expose the old `/admin/api.php` metrics endpoint.
- If you want Pi-hole metrics in Prometheus, you need a Pi-hole v6 compatible exporter.

---

## Grafana Won't Connect to Prometheus

**Symptom**: "Error connecting to Prometheus" in Grafana

**Fix**:
1. Go to Grafana (http://localhost:3000)
2. Settings → Data Sources
3. Click on Prometheus
4. Change URL from `http://localhost:9090` to `http://prometheus:9090`
5. Test Connection
6. Save

---

## High CPU or Memory Usage

**Symptom**: Computer slow, high resource usage

**Check**:
```bash
# See what's using resources
docker stats

# See host system
top                    # Linux/Mac
Get-Process | Sort-Object WS -Descending  # Windows
```

**Solution**:
1. Stop unused containers: `docker compose -f docker/docker-compose.yml down`
2. Remove old Docker data: `docker system prune`
3. Check disk space: `df -h` (Linux) or `Get-Volume` (Windows)

---

## Port Already in Use

**Symptom**: "Address already in use" error

**Find what's using the port**:
```bash
# Windows
netstat -ano | findstr :9000
taskkill /PID <PID> /F

# Linux
sudo lsof -i :9000
sudo kill -9 <PID>
```

**Or change the port** in `docker/docker-compose.yml`:
```yaml
ports:
  - "9001:9000"  # Use 9001 instead of 9000
```

Then: `docker compose -f docker/docker-compose.yml up -d`

---

## Docker Compose Errors

**"Cannot connect to Docker daemon"**
```bash
# Start Docker Desktop (on Windows/Mac)
# Or restart Docker on Linux:
sudo systemctl restart docker
```

**YAML syntax error**:
```bash
# Check syntax
docker compose -f docker/docker-compose.yml config

# Verify indentation is correct (2 spaces, not tabs)
# In docker-compose.yml
```

---

## Getting Help

### Check Logs
```bash
# See last 50 lines of all logs
docker compose -f docker/docker-compose.yml logs --tail 50

# Follow logs in real-time (Ctrl+C to stop)
docker compose -f docker/docker-compose.yml logs -f

# Specific service
docker compose -f docker/docker-compose.yml logs -f pihole
```

### Useful Commands
```bash
# List all containers
docker compose -f docker/docker-compose.yml ps

# See what images are running
docker images

# See storage volumes
docker volume ls

# Inspect a container
docker inspect <container_name>

# Get inside a container
docker exec -it <container_name> /bin/bash

# See network info
docker network ls
docker network inspect labnet
```

### Restart Everything (Clean Slate)
```bash
# Stop and remove everything
docker compose -f docker/docker-compose.yml down -v    # -v removes data too!

# Start fresh
docker compose -f docker/docker-compose.yml up -d

# Check status
docker compose -f docker/docker-compose.yml ps
```

---

## Still Stuck?

1. **Google the error message** - Usually finds the solution
2. **Check Docker logs** - They're usually helpful
3. **Restart Docker completely** - Fixes 50% of issues
4. **Check Docker Desktop** - Make sure it's actually running
5. **Verify .env file** - Check passwords are correct
6. **Check free disk space** - Docker needs space to work

---

## Common Fixes Summary

| Problem | Quick Fix |
|---------|-----------|
| Services won't start | `docker compose -f docker/docker-compose.yml logs` to see error |
| Can't access services | `docker compose -f docker/docker-compose.yml ps` - are they running? |
| Forgot password | Restart the container |
| High CPU | `docker stats` to see what's using it |
| Port in use | Change port in docker-compose.yml |
| Docker won't start | Restart Docker Desktop |
| No data in Prometheus | Check prometheus.yml exists, restart service |
| Grafana can't find Prometheus | Use `http://prometheus:9090` not localhost |

