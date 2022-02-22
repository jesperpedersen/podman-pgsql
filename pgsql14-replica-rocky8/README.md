# PostgreSQL Replica

This project contains the PostgreSQL Replica image.

## Getting Started

```bash
# Create the image
make build

# Run, and register the container under postgresql-replica
podman run -p 5433:5432 -p 9101:9100 --name postgresql-replica -d -e PG_MASTER=192.168.1.12 -e PG_REPLICATION_NAME=repl -e PG_REPLICATION_PASSWORD=replpass -e PG_SLOT_NAME=replica1 pgsql14-replica-rocky8

# psql to postgresql-replica
psql -h localhost -p 5433 -U myuser -c 'SELECT pg_is_in_recovery();' mydb

# Shell to postgresql-replica
podman exec -it postgresql-replica /usr/bin/bash

# Stop the container
podman stop postgresql-replica

# Start the container
podman start postgresql-replica

# Remove the container
podman rm postgresql-replica
```

## Configuration

| Property | Default | Unit | Required | Description |
|----------|---------|------|----------|-------------|
| PG_MASTER | | | Yes | The IP of the master |
| PG_REPLICATION_NAME | | | Yes | The replication user |
| PG_REPLICATION_PASSWORD | | | Yes | The password for the replication user |
| PG_SLOT_NAME | | | Yes | The replication slot name |

## SSL support

SSL support will be enabled when `/pgconf` contains the files `root.crt`, `postgresql.crt` and `postgresql.key`.

Remember to disable passphase such that the server can boot without a password prompt.

A guide to this can be found [here](https://www.howtoforge.com/postgresql-ssl-certificates).
Test and production environments should **NOT** be using self-signed certificates.
