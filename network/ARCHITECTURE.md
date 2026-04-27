# Network Architecture

This is a simple, local Docker-based lab. Everything runs on your computer.

## Simple Architecture

```text
Your Computer
|- Port 9000 -> Portainer (container management)
|- Port 80   -> Pi-hole (DNS blocking UI)
|- Port 53   -> Pi-hole (DNS)
|- Port 9090 -> Prometheus (metrics database)
|- Port 3000 -> Grafana (dashboards)
\- Port 8080 -> cAdvisor (container metrics UI)
```

## How They Talk to Each Other

```text
Grafana
  \- reads metrics from Prometheus

Prometheus
  |- scrapes itself at prometheus:9090
  |- scrapes host metrics from node-exporter:9100
  \- scrapes container metrics from cadvisor:8080

Portainer
  \- manages Docker through the Docker socket

Pi-hole
  |- blocks DNS requests on port 53
  \- serves its admin UI on port 80
```

## Data Flow Example

### When You View a Grafana Dashboard

```text
1. You open Grafana.
2. Grafana asks Prometheus for time-series data.
3. Prometheus returns metrics collected from exporters.
4. Grafana draws the dashboard panels.
```

### When Prometheus Collects Metrics

```text
1. Prometheus wakes up every 15 seconds.
2. It scrapes Node Exporter for host metrics.
3. It scrapes cAdvisor for container metrics.
4. It stores the samples in its local time-series database.
```

### When Pi-hole Blocks an Ad

```text
1. A device asks Pi-hole to resolve a domain.
2. Pi-hole checks its blocklists.
3. If the domain is blocked, Pi-hole blocks or null-routes the answer.
4. The blocked query appears in the Pi-hole admin UI.
```

## Port Explanation

```text
Port 53   = DNS queries handled by Pi-hole
Port 80   = Pi-hole web interface
Port 9000 = Portainer web interface
Port 3000 = Grafana web interface
Port 9090 = Prometheus web interface
Port 8080 = cAdvisor web interface
```

## Storage

Services that need persistent state use Docker volumes:

```text
portainer_data  -> Portainer settings
pihole_data     -> Pi-hole config and blocklists
pihole_dnsmasq  -> Pi-hole DNS config
prometheus_data -> collected metrics
grafana_data    -> dashboards, alerts, and Grafana state
```

Node Exporter and cAdvisor are mostly metric readers, so they do not need persistent volumes.

## Network Name: `labnet`

All containers are connected to a Docker bridge network called `labnet`. This lets containers reach each other by service name:

- Grafana talks to Prometheus at `http://prometheus:9090`
- Prometheus talks to Node Exporter at `http://node-exporter:9100`
- Prometheus talks to cAdvisor at `http://cadvisor:8080`

## Summary

- Simple: runs locally with Docker Compose
- Connected: services share the `labnet` Docker network
- Persistent: important app data is saved in Docker volumes
- Observable: host and container metrics now flow into Prometheus
