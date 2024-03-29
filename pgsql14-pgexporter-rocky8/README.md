# PostgreSQL postgres_exporter

This project contains the PostgreSQL pgexporter image.

## Getting Started

```bash
# Create the image
make build

# Run, and register the container under postgresql-pgexporter
podman run -p 3000:3000 --name postgresql-pgexporter -d -e PG_SERVER_NAME=192.168.1.12 -e PG_SERVER_PORT=5432 -e PG_MONITOR_NAME=monuser -e PG_MONITOR_PASSWORD=monpass pgsql14-pgexporter-rocky8

# Shell to postgresql-
podman exec -it postgresql-pgexporter /usr/bin/bash

# Stop the container
podman stop postgresql-pgexporter

# Start the container
podman start postgresql-pgexporter

# Remove the container
podman rm postgresql-pgexporter
```

## Configuration

| Property | Default | Unit | Required | Description |
|----------|---------|------|----------|-------------|
| PG_SERVER_NAME | | | Yes | The IP of the PostgreSQL server |
| PG_SERVER_PORT | | | Yes | The port of the PostgreSQL server |
| PG_MONITOR_NAME | | | Yes | The name of the monitor user |
| PG_MONITOR_PASSWORD | | | Yes | The password of the monitor user |
