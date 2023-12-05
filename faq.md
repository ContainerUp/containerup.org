---
title: FAQ
layout: default
nav_order: 40
---

# FAQ
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## Does this project support any other container runtimes?

[Podman](https://podman.io/){:target="_blank" .external} is the only supported runtime.

---

## What are the supported versions of Podman?

ContainerUp supports Podman v3 and v4. Some features can not work perfectly with Podman v3, however.

We suggest you to upgrade to Podman v4. To do that, you may need to upgrade your Linux distribution.

---

## What is a restart policy?

Containers will be restarted when they exit, if the restart policy is set to `always`.

Containers will also **be restarted after a system reboot**,
if systemd service `podman-restart.service` is enabled on your system.

This feature is available since Podman v3.3.0.
On Debian and Ubuntu, it's available since Debian 11 bookworm, and Ubuntu 22.04 jammy.

Refer to [documents of Podman](https://docs.podman.io/en/latest/markdown/podman-run.1.html#restart-policy){:target="_blank" .external}.

---

# Troubleshoot

## Cannot connect to WebSocket

[WebSocket](https://en.wikipedia.org/wiki/WebSocket){:target="_blank" .external} is used in ContainerUp,
so that the latest status of Podman reflects on your browser, without refreshing the page.

{: .warning }
It can be very **dangerous** to expose ContainerUp to the Internet without proper **security measures**.

Sometimes some special configurations on the web servers may be required, however,
if you access ContainerUp through a reverse proxy. 

[NGINX: WebSocket proxying](https://nginx.org/en/docs/http/websocket.html){:target="_blank" .external}

---

## The statistics data stream is ended

There are usually two possibilities.

### I see this notice on the statistics page of a stopped container

If everything works correctly, but **only see this notice when you stop a container**,
you may be using Podman v3, an out-dated version. 

As a limitation of Podman v3, statistics data streams aren't available on a stopped container.
Although ContainerUp supports Podman v3, it's not very worthwhile to work around this.

We suggest you to upgrade to Podman v4. To do that, you may need to upgrade your Linux distribution.

### I see this notice immediately after opening the overview page or the container statistics page

Please check the logs of Podman first. You may run this command:

```shell
journalctl -u podman -n --no-pager
```

If you see something like this:

![podman logs with cgroup errors](/assets/images/troubleshoot/cgroup_err.png)

`Failed to retrieve cgroup stats: open /sys/fs/cgroup/machine.slice/libpod-7ee2169140bc3a3903f5e6040624809d8edb66d604f305658b2ef8229eefb175.scope/container/memory.current: no such file or directory`

Check the status of **cgroups**. You may run this command:

```shell
cat /proc/cgroups
```

Then you will see the result like this: 

![cgroups_status](/assets/images/troubleshoot/cgroup_status.png){: width="400" }


```text
#subsys_name  hierarchy  num_cgroups  enabled
...
memory	0	46	0
...
```

As you can see, the **memory controller** of cgroup v2 is **disabled**.

This is the default configuration on **Raspberry Pi**.
To fix this, add `cgroup_memory=1 cgroup_enable=memory` to `/boot/cmdline.txt` (all contents should be in one line),
and reboot.

Refer to [GitHub raspberrypi/linux#3644](https://github.com/raspberrypi/linux/issues/3644#issuecomment-698546912){:target="_blank" .external}.

---

## Updating ContainerUp

v0.1.0
{: .label .label-purple }

### Why can't I update with just a click on the web page?

Currently, only containerized installation can be updated with just a click on the web page.
It's recommended to install ContainerUp [using a Podman container]({% link installation.md %}#install-using-containers).

### Container named XXXX exists. Make sure these containers are removed: XXXX, XXXX, XXXX

This message implies previously failed update. This may be caused by a bug.
These containers are kept for debugging purpose.

Remove the mentioned containers before you retry the update.

