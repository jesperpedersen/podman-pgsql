# PostgreSQL pgadmin4

This project contains the PostgreSQL pgadmin4 image.

## Getting Started

```bash
# Create the image
make build

# Run, and register the container under postgresql-pgadmin4
podman run --name postgresql-pgadmin4 -p 5050:5050 -d -e PGADMIN_SETUP_EMAIL=myaccount@acme.com -e PGADMIN_SETUP_PASSWORD=mypass pgsql12-pgadmin4-centos8

# Shell to postgresql-pgadmin4
podman exec -it postgresql-pgadmin4 /usr/bin/bash

# Stop the container
podman stop postgresql-pgadmin4

# Start the container
podman start postgresql-pgadmin4

# Remove the container
podman rm postgresql-pgadmin4
```

## Configuration

| Property | Default | Unit | Required | Description |
|----------|---------|------|----------|-------------|
| PGADMIN_SETUP_EMAIL | | | Yes | The email of the administrator |
| PGADMIN_SETUP_PASSWORD | | | Yes | The password of the administrator |

## Volumes

| Name | Description |
|------|-------------|
| `/pgconf` | Volume for configuration |
