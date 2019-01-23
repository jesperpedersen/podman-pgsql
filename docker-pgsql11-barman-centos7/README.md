# Docker / PostgreSQL Barman

This project contains the PostgreSQL Barman image for Docker.

## Getting Started

```bash
su -

# Start the docker daemon
systemctl start docker

# Create the docker image
make build

# Run, and register the container under postgresql-barman
docker run --name postgresql-barman -d -e PG_MASTER=172.17.0.2 -e PG_DATABASE=mydb -e PG_REPLICATION_PASSWORD=replpass docker-pgsql11-barman-centos7

# Shell to postgresql-barman
docker exec -it postgresql-barman /usr/bin/bash

# Stop the container
docker stop postgresql-barman

# Start the container
docker start postgresql-barman

# Remove the container
docker rm postgresql-barman
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
