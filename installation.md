---
title: Installation
layout: default
nav_order: 20
---

# Installation

## Prerequisite

You should have Podman installed on your system.

For details, you can refer to
[Podman Installation Instructions](https://podman.io/docs/installation#installing-on-linux){: target="_blank" .external }.

### Debian / Ubuntu

```shell
apt install podman
```

### Fedora

```shell
dnf install podman
```

## Release

ContainerUp is released on [GitHub](https://github.com/ContainerUp/containerup/releases){: target="_blank" .github}.

Images of ContainerUp are hosted on [Quay.io](https://quay.io/repository/containerup/containerup?tab=info){: target="_blank" .external}.

Two methods of installation is provided here.

{: .info }
It's recommended to install ContainerUp [using a Podman container](#install-using-containers).

## Install using binaries

Binaries of ContainerUp can be downloaded on the release page. Choose the file according to your architecture.

```
$ ./containerup -h
Usage of containerup:
  -generate-hash
        Generate a hash from your password, then exit. For security reasons, you have to input your password interactively.
  -listen string
        Address to listen. (default "127.0.0.1:3876")
  -password-hash hash
        REQUIRED. The bcrypt hash of password to be used on the web. Generate a password hash by using argument --generate-hash
  -podman URL
        URL of Podman. (default "unix:/run/podman/podman.sock")
  -tls-cert string
        Path of TLS certificate
  -tls-key string
        Path of TLS key
  -username string
        The username to be used on the web. (default "podman")
  -v3
        Connect to Podman with a v3 legacy version.
  -version
        Show the version of ContainerUp, then exit.
```

## Install using containers

ContainerUp has been tested with root user only. **Run all the commands with root user.**

### Environment variables

Check [this page]({% link configurations.md %}#environment-variables-of-container).

### Example commands

Remember to **mount the socket of Podman API** into the container, and **set your password hash**.

```shell
# Pull the image
podman pull quay.io/containerup/containerup:latest

# Check the version of your Podman
podman version

# Generate your password hash
podman run --rm -it quay.io/containerup/containerup:latest containerup -generate-hash
# example input 12345, output:
# $2a$10$tRhTPH7xGTJnNUUWgH/96.klhqU2z7zEPTwqa0/KfzJa4RHrVQF0O

# Run ContainerUp
# If you're running Podman v3, add an environment variable CONTAINERUP_PODMAN_V3=1
podman run -d --name containerup -p 3876:3876 \
  --restart always \
  -v /run/podman/podman.sock:/run/podman/podman.sock \
  -e CONTAINERUP_USERNAME=admin \
  -e CONTAINERUP_PASSWORD_HASH='$2a$10$tRhTPH7xGTJnNUUWgH/96.klhqU2z7zEPTwqa0/KfzJa4RHrVQF0O' \
  quay.io/containerup/containerup:latest
```
