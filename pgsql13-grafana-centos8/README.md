# PostgreSQL Grafana

This project contains the PostgreSQL Grafana/Prometheus/TimescaleDB image.

## Getting Started

```bash
# Create the image
make build

# Run, and register the container under postgresql-grafana
podman run -p 9090:9090 -p 3000:3000 --name postgresql-grafana -d -e PG_USER_NAME=monuser -e PG_USER_PASSWORD=monpass -e PG_NETWORK_MASK=all -e PG_MONITOR1=192.168.1.12:9100 -e PG_MONITOR2=192.168.1.12:9187 pgsql13-grafana-centos8

# Shell to postgresql-grafana
podman exec -it postgresql-grafana /usr/bin/bash

# Get the IP address of the postgresql-grafana container
podman inspect postgresql-grafana | grep IPAddress

# Stop the container
podman stop postgresql-grafana

# Start the container
podman start postgresql-grafana

# Remove the container
podman rm postgresql-grafana
```

## Configuration

| Property | Default | Unit | Required | Description |
|----------|---------|------|----------|-------------|
| PG_USER_NAME | | | Yes | The user name |
| PG_USER_PASSWORD | | | Yes | The password for the user |
| PG_NETWORK_MASK | | | Yes | The network mask for database access |
| PG_MAX_CONNECTIONS | 100 | | | `max_connections` setting |
| PG_SHARED_BUFFERS | 256 | MB | | `shared_buffers` setting |
| PG_WORK_MEM | 8 | MB | | `work_mem` setting |
| PG_MAX_PARALLEL_WORKERS | 8 | | | `max_parallel_workers` setting |
| PG_EFFECTIVE_CACHE_SIZE | 1 | GB | | `effective_cache_size` setting |
| PG_MAX_WAL_SIZE | 1 | GB | | `max_wal_size` setting |
| PG_MONITOR1 | | | Yes | The first monitoring point |
| PG_MONITOR2 | | | | The second monitoring point |
| PG_MONITOR3 | | | | The third monitoring point |
| PG_MONITOR4 | | | | The fourth monitoring point |

## Volumes

| Name | Description |
|------|-------------|
| `/pgconf` | Volume for SSL configuration |
| `/pgdata` | PostgreSQL data directory |
| `/pgwal` | PostgreSQL Write-Ahead Log (WAL) |

## SSL support

SSL support will be enabled when `/pgconf` contains the files `root.crt`, `server.crt` and `server.key`.

Remember to disable passphase such that the server can boot without a password prompt.

A guide to this can be found [here](https://www.howtoforge.com/postgresql-ssl-certificates).
Test and production environments should **NOT** be using self-signed certificates.
