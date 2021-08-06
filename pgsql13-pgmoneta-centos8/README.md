# PostgreSQL pgmoneta

This project contains the PostgreSQL pgmoneta image.

## Getting Started

```bash
# Create the image
make build

# Run, and register the container under postgresql-pgmoneta
podman run -p 5001:5001 -p 9102:9100 --name postgresql-pgmoneta -d -e PG_PRIMARY_NAME=192.168.1.12 -e PG_PRIMARY_PORT=5432 -e PG_REPL_NAME=repl -e PG_REPL_PASSWORD=mypass -e PG_WAL_NAME=backup pgsql13-pgmoneta-centos8

# Shell to postgresql-
podman exec -it postgresql-pgmoneta /usr/bin/bash

# Stop the container
podman stop postgresql-pgmoneta

# Start the container
podman start postgresql-pgmoneta

# Remove the container
podman rm postgresql-pgmoneta
```

## Configuration

| Property | Default | Unit | Required | Description |
|----------|---------|------|----------|-------------|
| PG_PRIMARY_NAME | | String | Yes | The IP of the PostgreSQL server |
| PG_PRIMARY_PORT | | String | Yes | The port of the PostgreSQL server |
| PG_BACKUP_NAME | | String | Yes | The replication user name |
| PG_BACKUP_PASSWORD | | String | Yes | The replication user password |
| PG_BACKUP_SLOT | | String | Yes | The name of the WAL slot |

## SSL support

SSL support will be enabled when `/pgconf` contains the files `root.crt`, `server.crt` and `server.key`.

Remember to disable passphase such that the server can boot without a password prompt.

A guide to this can be found [here](https://www.howtoforge.com/postgresql-ssl-certificates).
Test and production environments should **NOT** be using self-signed certificates.
