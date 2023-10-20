---
title: FAQ
layout: default
nav_order: 40
---

# FAQ

## Does this project support any other container runtimes?

[Podman](https://podman.io/){:target="_blank" .external} is the only supported runtime.

## What is a restart policy?

Containers will be restarted when they exit, if the restart policy is set to `always`.

Containers will also **be restarted after a system reboot**,
if systemd service `podman-restart.service` is enabled on your system.

This feature is available since Podman v3.3.0.
On Debian and Ubuntu, it's available since Debian 11 bookworm, and Ubuntu 22.04 jammy.

Refer to [documents of Podman](https://docs.podman.io/en/latest/markdown/podman-run.1.html#restart-policy).

# Troubleshoot

## Cannot connect to WebSocket

[WebSocket](https://en.wikipedia.org/wiki/WebSocket){:target="_blank" .external} is used in ContainerUp,
so that the latest status of Podman reflects on your browser, without refreshing the page.

{: .warning }
It can be very **dangerous** to expose ContainerUp to the Internet without proper **security measures**.

Sometimes some special configurations on the web servers may be required, however,
if you access ContainerUp through a reverse proxy. 

[NGINX: WebSocket proxying](https://nginx.org/en/docs/http/websocket.html){:target="_blank" .external}

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

[GitHub raspberrypi/linux#3644](https://github.com/raspberrypi/linux/issues/3644#issuecomment-698546912){:target="_blank" .external}
