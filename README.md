# PostgreSQL

This repository contains [PostgreSQL](https://www.postgresql.org) images for [podman](https://podman.io/).
[podman](https://podman.io/) 1.2+ is required.

The images makes use of

* [CentOS](https://www.centos.org) 8
* [PostgreSQL](https://www.postgresql.org) 13.x
* Community provided RPMs through [yum.postgresql.org](https://yum.postgresql.org)
* Checksum for `PGDATA`
* SCRAM-SHA256 password encryption by default
* SSL support
* `pg_stat_statements` integration
* Pooling using [pgagroal](https://agroal.github.io/pgagroal/)
* Backup using [barman](https://www.pgbarman.org/)
* Administration with [pgadmin4](https://www.pgadmin.org/)
* Monitoring with [Grafana](https://grafana.com/), [Prometheus](https://prometheus.io/) and [TimescaleDB](https://www.timescale.com/)
* Asynchronous replication, up to 5 slaves
* Volumes for `/pgconf`, `/pgdata`, `/pgwal` and `/pgbackup`.

Features that should be added

* Synchronous replication with `remote_apply`
* Additional GUCs

Images are provided **`AS IS`** according to the license agreement with
no guarantees of correctness and stability.

## License

All images are released under the MIT license.

## Author

Jesper Pedersen

jesper (dot) pedersen (at) redhat (dot) com
