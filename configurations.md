---
title: Configurations
layout: default
nav_order: 30
---

# Configurations

## Environment variables of container

### CONTAINERUP_PASSWORD_HASH

**Required**

`CONTAINERUP_PASSWORD_HASH` is the SHA256 hash of your username and password.
Your can generate one with the following command:

```shell
echo -n <username>:<password> | sha256sum
```

### CONTAINERUP_PODMAN_V3

Optional

Set `CONTAINERUP_PODMAN_V3` to 1 if you are using Podman v3.

