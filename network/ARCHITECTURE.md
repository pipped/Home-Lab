# Network Architecture

This is a simple, local Docker-based lab. Everything runs on your computer.

## Simple Architecture

```
Your Computer
├── Port 9000  → Portainer (Container Management)
├── Port 80    → Pi-hole (DNS Blocking)
├── Port 9090  → Prometheus (Metrics Database)
└── Port 3000  → Grafana (Dashboards)
```

## How They Talk to Each Other

```
Grafana (3000)
   └─→ Asks Prometheus for data
       
Prometheus (9090)
   └─→ Stores metrics from containers
   
Portainer (9000)
   └─→ Shows you all containers
   
Pi-hole (80)
   └─→ Blocks ad DNS requests
   └─→ Exposes metrics to Prometheus
```

## Data Flow Example

### When You View a Grafana Dashboard

```
1. You open Grafana
   ↓
2. Grafana asks Prometheus: "How much memory is being used?"
   ↓
3. Prometheus checks its database: "Container used 500MB 5 mins ago, 512MB now"
   ↓
4. Prometheus returns data to Grafana
   ↓
5. Grafana draws a pretty graph
   ↓
6. You see the line going up and down
```

### When Pi-hole Blocks an Ad

```
1. Your browser tries to load an ad domain
   ↓
2. Browser asks Pi-hole (DNS): "Where is ads.badsite.com?"
   ↓
3. Pi-hole checks its blocklist: "That's an ad! Block it."
   ↓
4. Pi-hole doesn't respond / returns empty
   ↓
5. Browser never loads the ad
   ↓
6. You see "Ad blocked" in Pi-hole admin
```

## Port Explanation

```
Port 80    = HTTP (web)
Port 9000  = Portainer web interface
Port 3000  = Grafana web interface
Port 9090  = Prometheus web interface
Port 53    = DNS (internal, not accessible)
```

## Storage (Volumes)

Each service saves data so it doesn't lose everything when you restart:

```
portainer_data     → Portainer settings
pihole_data        → Pi-hole blocklists & config
prometheus_data    → All collected metrics
grafana_data       → Your dashboards & alerts
```

## Network Name: "labnet"

All containers are connected to a Docker network called `labnet`. This lets them talk to each other:

- Grafana talks to Prometheus on: `http://prometheus:9090`
- Prometheus talks to Pi-hole on: `http://pihole:80`
- Etc.

---

## Summary

✓ **Simple**: 4 services running locally  
✓ **Connected**: All talk to each other  
✓ **Persistent**: Data saved in volumes  
✓ **Learning**: Great for understanding Docker & monitoring  

That's it! No complex network setup, no VMs, no firewall rules.

