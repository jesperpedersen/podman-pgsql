# Podman

[Podman](https://podman.io/) can be used instead of `docker` for the containers.
[Buildah](https://buildah.io/) is used as the image build tool.

## Getting Started

```bash
su -

# Start the CRI-O daemon
systemctl start crio

# Create the image
buildah bud -t podman-pgsql11-primary-centos7 .

# Run, and register the container under pgsql11-primary
podman run --name pgsql11-primary -e PG_DATABASE=mydb -e PG_USER_NAME=myuser -e PG_USER_PASSWORD=mypass -e PG_REPLICATION_NAME=repl -e PG_REPLICATION_PASSWORD=replpass -e PG_NETWORK_MASK=all -d -p 5432:5432 podman-pgsql11-primary-centos7:latest

# psql to pgsql11-primary
psql -h localhost -p 5432 -U myuser mydb

# Shell to pgsql11-primary
podman exec -t pgsql11-primary /bin/bash

# Get the IP address of the pgsql11-primary container
podman inspect pgsql11-primary | grep IPAddress

# Stop the container
podman stop pgsql11-primary

# Start the container
podman start pgsql11-primary

# Remove the container
podman rm pgsql11-primary
```

## Rootless mode

[Podman](https://podman.io/) can also be run as a non-root user.

The only change to the above is the `podman run` command which needs an additional parameter
of `--net=host`, as container port bindings isn't supported yet.
