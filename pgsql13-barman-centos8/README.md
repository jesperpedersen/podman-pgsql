# PostgreSQL Barman

This project contains the PostgreSQL Barman image.

## Getting Started

```bash
# Create the image
make build

# Run, and register the container under postgresql-barman
podman run -p 9102:9100 --name postgresql-barman -d -e PG_MASTER=192.168.1.12 -e PG_DATABASE=mydb -e PG_REPLICATION_PASSWORD=replpass pgsql13-barman-centos8

# Shell to postgresql-barman
podman exec -it postgresql-barman /usr/bin/bash

# Stop the container
podman stop postgresql-barman

# Start the container
podman start postgresql-barman

# Remove the container
podman rm postgresql-barman
```

## Configuration

| Property | Default | Unit | Required | Description |
|----------|---------|------|----------|-------------|
| PG_MASTER | | | Yes | The IP of the master |
| PG_DATABASE | | | Yes | The name of the database |
| PG_REPLICATION_PASSWORD | | | Yes | The password for the replication user |

## Volumes

| Name | Description |
|------|-------------|
| `/pgconf` | Volume for configuration |
| `/pgbackup` | Volume for backup |
