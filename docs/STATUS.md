# Current Status

This document tracks what is working in the repo right now and what still needs optional follow-up.

## Working

- Docker Compose starts the core lab services from `docker/docker-compose.yml`
- Pi-hole v6 is configured with the current `FTLCONF_` environment variables
- Grafana is provisioned with a Prometheus datasource
- Grafana is provisioned with a starter dashboard:
  - Folder: `Home Lab`
  - Dashboard: `Home Lab Overview`
- Prometheus loads its config successfully from `configs/prometheus.yml`

## Important Notes

- Pi-hole v6 changed both its Docker environment variables and its API surface.
- The old unauthenticated endpoint `/admin/api.php` is not used in this repo anymore.
- Prometheus does not scrape Pi-hole directly because Pi-hole v6 exposes authenticated JSON API endpoints instead of a Prometheus scrape endpoint.

## Current Monitoring Limits

- `prometheus` is available and queryable
- `grafana` can visualize Prometheus data
- `docker` metrics are not active until Docker metrics are explicitly enabled or an exporter is added
- `node` metrics are not active until a node exporter is added
- `pihole` metrics require a Pi-hole v6 compatible exporter

## Recommended Next Additions

1. Add a node exporter for host CPU, memory, and disk metrics.
2. Add Docker metrics or a container exporter for per-container graphs.
3. Add a Pi-hole v6 exporter if you want Pi-hole stats in Grafana.
