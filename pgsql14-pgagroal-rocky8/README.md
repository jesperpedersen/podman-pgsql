# PostgreSQL pgagroal

This project contains the PostgreSQL pgagroal image.

## Getting Started

```bash
# Create the image
make build

# Run, and register the container under postgresql-pgagroal
podman run -p 2345:2345 -p 2346:2346 -p 9104:9100 --name postgresql-pgagroal -d -e PG_PRIMARY_NAME=192.168.1.12 -e PG_PRIMARY_PORT=5432 -e PG_NETWORK_MASK=all -e PG_USER1_NAME=myuser -e PG_USER1_PASSWORD=mypass -e PG_USER1_DATABASE=mydb pgsql14-pgagroal-rocky8

# Shell to postgresql-
podman exec -it postgresql-pgagroal /usr/bin/bash

# Stop the container
podman stop postgresql-pgagroal

# Start the container
podman start postgresql-pgagroal

# Remove the container
podman rm postgresql-pgagroal
```

## Configuration

| Property | Default | Unit | Required | Description |
|----------|---------|------|----------|-------------|
| PG_PRIMARY_NAME | | String | Yes | The IP of the PostgreSQL server |
| PG_PRIMARY_PORT | | String | Yes | The port of the PostgreSQL server |
| PG_NETWORK_MASK | | String | Yes | The network mask for access |
| PG_USER1_NAME | | String | Yes | The user name |
| PG_USER1_PASSWORD | | String | Yes | The user password |
| PG_USER1_DATABASE | | String | Yes | The user database |
| PG_MAX_CONNECTIONS | 100 | Int | No | The maximum number of connections |
| PG_IDLE_TIMEOUT | 0 | Int | No | The idle timeout setting |
| PG_VALIDATION | `off` | Bool | No | The validation of connections |

## SSL support

SSL support will be enabled when `/pgconf` contains the files `root.crt`, `server.crt` and `server.key`.

Remember to disable passphase such that the server can boot without a password prompt.

A guide to this can be found [here](https://www.howtoforge.com/postgresql-ssl-certificates).
Test and production environments should **NOT** be using self-signed certificates.
