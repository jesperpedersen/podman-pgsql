# PostgreSQL Primary

This project contains the PostgreSQL Primary image.

## Getting Started

```bash
# Create the image
make build

# Run, and register the container under postgresql-primary
podman run -p 5432:5432 -p 9100:9100 --name postgresql-primary -d -e PG_DATABASE=mydb -e PG_USER_NAME=myuser -e PG_USER_PASSWORD=mypass -e PG_REPLICATION_NAME=repl -e PG_REPLICATION_PASSWORD=replpass -e PG_BACKUP_NAME=backup  -e PG_BACKUP_PASSWORD=backuppass -e PG_BACKUP_SLOT=backup -e PG_NETWORK_MASK=all -e PG_MONITOR_NAME=monuser -e PG_MONITOR_PASSWORD=monpass pgsql14-primary-rocky8

# psql to postgresql-primary
psql -h localhost -p 5432 -U myuser mydb

# Shell to postgresql-primary
podman exec -it postgresql-primary /usr/bin/bash

# Get the IP address of the postgresql-primary container; if empty use host IP address
podman inspect postgresql-primary | grep IPAddress

# Stop the container
podman stop postgresql-primary

# Start the container
podman start postgresql-primary

# Remove the container
podman rm postgresql-primary
```

## Configuration

| Property | Default | Unit | Required | Description |
|----------|---------|------|----------|-------------|
| PG_DATABASE | | | Yes | The name of the database |
| PG_USER_NAME | | | Yes | The user name |
| PG_USER_PASSWORD | | | Yes | The password for the user |
| PG_REPLICATION_NAME | | | Yes | The replication user |
| PG_REPLICATION_PASSWORD | | | Yes | The password for the replication user |
| PG_BACKUP_NAME | | | Yes | The backup user |
| PG_BACKUP_PASSWORD | | | Yes | The password for the backup user |
| PG_BACKUP_SLOT | | | Yes | The WAL slot for backup |
| PG_NETWORK_MASK | | | Yes | The network mask for database access |
| PG_DATABASE_ENCODING | UTF8 | | | The encoding of the database |
| PG_MAX_CONNECTIONS | 100 | | | `max_connections` setting |
| PG_SHARED_BUFFERS | 256 | MB | | `shared_buffers` setting |
| PG_WORK_MEM | 8 | MB | | `work_mem` setting |
| PG_MAX_PARALLEL_WORKERS | 8 | | | `max_parallel_workers` setting |
| PG_EFFECTIVE_CACHE_SIZE | 1 | GB | | `effective_cache_size` setting |
| PG_MAX_WAL_SIZE | 1 | GB | | `max_wal_size` setting |
| PG_PASSWORD_ENCRYPTION | scram-sha-256 | | | `password_encryption` setting |
| PG_REPLICA1 | replica1 | | | Replication slot name 1 |
| PG_REPLICA2 | replica2 | | | Replication slot name 2 |
| PG_REPLICA3 | replica3 | | | Replication slot name 3 |
| PG_REPLICA4 | replica4 | | | Replication slot name 4 |
| PG_REPLICA5 | replica5 | | | Replication slot name 5 |
| PG_MONITOR_NAME | | | | The name of the monitor user |
| PG_MONITOR_PASSWORD | | | | The password of the monitor user |

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
