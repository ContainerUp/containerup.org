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

Images of ContainerUp are hosted on [Quay.io](https://quay.io/repository/containerup/containerup?tab=info).

Two methods of installation is provided here.

{: .info }
It's recommended to install ContainerUp [using a Podman container](#install-using-containers).

## Install using binaries

Binaries of ContainerUp can be downloaded on the release page. Choose the file according to your architecture.

## Install using containers

ContainerUp has been tested with root user only. **Run all the commands with root user.**

### Environment variables

Check [this page]({% link installation.md %}).

### Example commands

Remember to **mound the socket of Podman API** into the container.

```shell
# Pull the image
podman pull quay.io/containerup/containerup:latest

# Check the version of your Podman
podman version

# Generate your password hash
echo -n <username>:<password> | sha256sum
# example output:
# bc842c31a9e54efe320d30d948be61291f3ceee4766e36ab25fa65243cd76e0e -

# Run ContainerUp
# If you're running Podman v3, add an environment variable CONTAINERUP_PODMAN_V3=1
podman run -d --name containerup -p 3876:3876 \
  --restart always \
  -v /run/podman/podman.sock:/run/podman/podman.sock \
  -e CONTAINERUP_PASSWORD_HASH="bc842c31a9e54efe320d30d948be61291f3ceee4766e36ab25fa65243cd76e0e" \
  quay.io/containerup/containerup:latest
```
