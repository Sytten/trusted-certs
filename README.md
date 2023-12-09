[![Docker version](https://img.shields.io/docker/v/sytten/trusted-certs?style=for-the-badge&label=Docker&color=blue)](https://hub.docker.com/r/sytten/trusted-certs)
![License](https://img.shields.io/badge/LICENSE-AGPLv3-green?style=for-the-badge)

# Trusted certs - Linuxserver.io mod

This docker mod helps you with automatically trusting new certificates that are added to a special trusted-certs config folder.

# How to

Add your certificate with an extension of a `.crt` to container's `/config/trusted-certs` folder, and as a result, it will be trusted within the docker container.

# Config

```yml
version: "2.1"
services:
  service-name:
    image: ghcr.io/linuxserver/service-name
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - DOCKER_MODS=sytten/trusted-certs # <---
    volumes:
      - /path/to/appdata/config:/config
    restart: unless-stopped
```

# Inner workings

The new certificates are copied to `/usr/share/ca-certificates` folder and appropriate entries are added to the `/etc/ca-certificates.conf` config file. After that the store is updated by running `update-ca-certificates` command.

# Caveats

The mod is not tested on all base images and thus might not work. As of right now, there is no way to detect the deletions of files from the `/config/trusted-certs` folder and automatically reflect those changes in the container. The certificate needs to be removed manually, or the container needs to be re-created after deleting a certificate from the folder.
