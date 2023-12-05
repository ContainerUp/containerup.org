---
title: Configurations
layout: default
nav_order: 30
---

# Configurations

## Environment variables of container

### `CONTAINERUP_USERNAME`

Optional
{: .label .label-yellow }

v0.1.0
{: .label .label-purple }

The username to be used on the web. Defaults to `podman`.

---

### `CONTAINERUP_PASSWORD_HASH`

Required
{: .label .label-red }

v0.1.0
{: .label .label-purple }

It's the bcrypt hash of your desired password to use in the web.
Your can generate one with the following command:

```shell
podman run --rm -it quay.io/containerup/containerup:latest containerup --generate-hash
```

---

### `CONTAINERUP_PODMAN_V3`

Optional
{: .label .label-yellow }

v0.1.0
{: .label .label-purple }

Set to `1` if you are using Podman v3.

---

### `CONTAINERUP_TLS_CERT`

Optional
{: .label .label-yellow }

v0.1.0
{: .label .label-purple }

Location of TLS certificate file. Must be used together with `CONTAINERUP_TLS_KEY`.

Certificate and key files must be mounted into the container.

---

### `CONTAINERUP_TLS_KEY`

Optional
{: .label .label-yellow }

v0.1.0
{: .label .label-purple }

Refer to `CONTAINERUP_TLS_CERT`.
