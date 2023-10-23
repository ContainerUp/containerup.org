---
title: Configurations
layout: default
nav_order: 30
---

# Configurations

## Environment variables of container

### `CONTAINERUP_USERNAME`

Optional

The username to be used on the web. Defaults to `podman`.

### `CONTAINERUP_PASSWORD_HASH`

Required
 
It's the bcrypt hash of your desired password to use in the web.
Your can generate one with the following command:

```shell
podman run --rm -it quay.io/containerup/containerup:latest containerup --generate-hash
```

### `CONTAINERUP_PODMAN_V3`

Optional

Set to 1 if you are using Podman v3.

