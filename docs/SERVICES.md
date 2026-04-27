# Services Guide

## Portainer - Container Management

**URL**: http://localhost:9000  
**Purpose**: Visual interface to manage Docker containers

### What You Can Do

- Start and stop containers
- View container logs
- Inspect volumes and networks
- Check basic resource usage

### Getting Started

1. Open `http://localhost:9000`
2. Create an admin password on first login
3. Click through the running containers to inspect them

---

## Pi-hole - DNS Ad Blocker

**URL**: http://localhost/admin  
**Password**: `PIHOLE_PASSWORD` from `.env`  
**Purpose**: Blocks ads at the DNS level

### What You Can Do

- Block DNS requests for ad and tracking domains
- View recent DNS queries
- Add allow and deny rules
- Review query counts and client activity

### Getting Started

1. Open `http://localhost/admin`
2. Sign in with the password from `.env`
3. Review the dashboard and query counts
4. Add blocklists if you want to experiment further

### Important Note

This repo uses Pi-hole v6. Its current API no longer matches the old `/admin/api.php` workflow that many older Docker examples still reference.

---

## Prometheus - Metrics Collection

**URL**: http://localhost:9090  
**Purpose**: Collects and stores metrics over time

### What You Can Do

- Query metrics directly
- See which scrape targets are up
- Check Prometheus health and ingestion

### Good First Queries

**Target health**
```promql
up
```

**Prometheus head series**
```promql
prometheus_tsdb_head_series
```

**Prometheus HTTP request rate**
```promql
rate(prometheus_http_requests_total[5m])
```

### Good Exporter Queries

**Host CPU**
```promql
rate(node_cpu_seconds_total[5m])
```

**Container memory**
```promql
container_memory_usage_bytes
```

**Container CPU**
```promql
rate(container_cpu_usage_seconds_total[5m])
```

### Important Note

Prometheus is working and now scrapes Node Exporter and cAdvisor. The `docker` scrape job is still a placeholder until Docker's native metrics endpoint is enabled, and Pi-hole metrics still need a Pi-hole v6 compatible exporter.

---

## Node Exporter - Host Metrics

**URL**: scraped internally at `http://node-exporter:9100`  
**Purpose**: Exposes host machine metrics to Prometheus

### What You Can See

- CPU usage
- Memory usage
- Disk and filesystem stats
- Network stats
- System load and uptime

### Getting Started

1. Open Prometheus at `http://localhost:9090`
2. Go to `Status -> Target health`
3. Confirm `node-exporter` is up
4. Try the query `node_uname_info`

---

## cAdvisor - Container Metrics

**URL**: http://localhost:8080  
**Purpose**: Exposes per-container resource metrics to Prometheus

### What You Can See

- Container CPU usage
- Container memory usage
- Container filesystem activity
- Container network traffic
- Container labels and metadata

### Getting Started

1. Open cAdvisor at `http://localhost:8080`
2. Open Prometheus at `http://localhost:9090`
3. Confirm the `cadvisor` target is up
4. Try the query `container_memory_usage_bytes`

---

## Grafana - Dashboards and Visualization

**URL**: http://localhost:3000  
**Login**: `admin` / `GF_SECURITY_ADMIN_PASSWORD` from `.env`  
**Purpose**: Visualize Prometheus data

### What You Can Do

- Browse dashboards
- Explore Prometheus queries visually
- Build your own dashboards and panels

### Getting Started

1. Open `http://localhost:3000`
2. Sign in with the password from `.env`
3. Open `Dashboards -> Home Lab -> Home Lab Overview`
4. Open `Explore` and run the query `up`

### Important Note

The Prometheus datasource is already provisioned automatically in this repo. You do not need to add it manually.

---

## Service Relationships

```text
Your browser
  |
  +-- Portainer (9000)
  +-- Pi-hole (80 for UI, 53 for DNS)
  +-- Prometheus (9090)
  +-- cAdvisor (8080)
  \-- Grafana (3000)
        |
        \-- reads data from Prometheus
```

Prometheus scrapes:

- itself at `prometheus:9090`
- Node Exporter at `node-exporter:9100`
- cAdvisor at `cadvisor:8080`

---

## Storage

Each service stores data in Docker volumes so it survives restarts:

- `portainer_data`
- `pihole_data`
- `pihole_dnsmasq`
- `prometheus_data`
- `grafana_data`

You can list them with:

```bash
docker volume ls
```

---

## Next Steps

1. Explore Portainer to inspect the running stack.
2. Open Prometheus and run `up`.
3. Open Grafana and review `Home Lab Overview`.
4. Build Grafana panels from Node Exporter and cAdvisor metrics.
5. Add a Pi-hole v6 exporter later if you want Pi-hole stats in Grafana.
