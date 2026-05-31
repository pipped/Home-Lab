# Current Status

## Working

- Docker Compose starts all services from `docker/docker-compose.yml`
- Pi-hole v6 with current `FTLCONF_` environment variables
- Pi-hole Exporter (`ekofr/pihole-exporter`) scrapes Pi-hole v6 API and exposes Prometheus metrics on port 9617
- Node Exporter exposes host CPU, memory, disk, and network metrics
- cAdvisor exposes per-container CPU, memory, filesystem, and network metrics
- Prometheus scrapes all four exporters: node-exporter, cadvisor, pihole-exporter, prometheus itself
- Grafana provisioned with Prometheus datasource
- Grafana provisioned with **Home Lab Overview** dashboard covering host metrics, container metrics, and Pi-hole stats
- Uptime Kuma available for service uptime monitoring (configure monitors manually at http://localhost:3001)
- Nginx Proxy Manager available for reverse proxy and SSL termination (configure at http://localhost:81)

## Port Map

| Port | Service |
|---|---|
| 53 | Pi-hole DNS |
| 80 | Nginx Proxy Manager (HTTP) |
| 81 | Nginx Proxy Manager (Admin UI) |
| 443 | Nginx Proxy Manager (HTTPS) |
| 3000 | Grafana |
| 3001 | Uptime Kuma |
| 8080 | cAdvisor |
| 8081 | Pi-hole Admin UI |
| 9000 | Portainer |
| 9090 | Prometheus |
| 9617 | Pi-hole Exporter (internal) |

## Known Limitations

- Uptime Kuma monitors must be added manually via the web UI — there is no provisioning file
- Nginx Proxy Manager SSL certificates require a real domain name and port 80/443 forwarded from your router
- The `docker` Prometheus job has been removed — Docker's native metrics endpoint requires manual daemon config
- Pi-hole exporter uses the web password for authentication; if the password is rotated in `.env`, restart the exporter container

## Recommended Next Steps

- Add monitors in Uptime Kuma for each service URL
- Configure Nginx Proxy Manager with your local domain for named access (e.g. `grafana.homelab.local`)
- Add Prometheus alert rules in `configs/prometheus.yml` once baseline metrics look stable
- Import Node Exporter Full dashboard from Grafana Labs (ID: 1860) for deeper host metrics
