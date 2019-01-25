# Docker / PostgreSQL

This repository contains [PostgreSQL](https://www.postgresql.org) images for [Docker](https://hub.docker.com/).
Alternative, [Podman](PODMAN.md) can be used.

The images makes use of

* [CentOS](https://www.centos.org) 7
* [PostgreSQL](https://www.postgresql.org) 11.x
* Community provided RPMs through [yum.postgresql.org](https://yum.postgresql.org)
* Checksum for `PGDATA`
* SCRAM-SHA256 password encryption by default
* SSL support
* `pg_stat_statements` integration
* Backup using [barman](https://www.pgbarman.org/)
* Asynchronous replication, up to 5 slaves
* Volumes for `/pgconf`, `/pgdata`, `/pgwal` and `/pgbackup`.

Features that should be added

* Synchronous replication with `remote_apply`
* Additional GUCs
* [pgpool-II 4.0 image](http://www.pgpool.net)

Images are provided **`AS IS`** according to the license agreement with
no guarantees of correctness and stability.

## License

All images are released under the MIT license.

## Author

Jesper Pedersen

jesper (dot) pedersen (at) redhat (dot) com
