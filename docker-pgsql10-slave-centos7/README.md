# Docker / PostgreSQL Slave

This project contains the PostgreSQL Slave image for Docker.

## Getting Started

```bash
su -
systemctl start docker
make build
docker run -p 5433:5432 -e PG_MASTER=172.12.0.2 -e PG_REPLICATION_NAME=repl -e PG_REPLICATION_PASSWORD=replpass -e PG_SLOT_NAME=replica1 docker-pgsql10-slave-centos7
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
