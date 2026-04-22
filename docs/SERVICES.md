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

### Important Note

Prometheus is working, but this repo does not yet include a node exporter, Docker exporter, or Pi-hole v6 exporter. That means the starter setup is best for learning the flow first, not for full host monitoring yet.

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
  \-- Grafana (3000)
        |
        \-- reads data from Prometheus
```

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
4. Add exporters later if you want richer host or Pi-hole metrics.
