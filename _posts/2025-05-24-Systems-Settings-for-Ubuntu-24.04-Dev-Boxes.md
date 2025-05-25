---
tags:
  - Linux
  - Ubuntu
  - systemd
  - limits
---
While Docker’s documentation does a great job covering installation and steps like enabling non-root usage, it tends to leave out important system-level configuration—like how to properly set file limits on Ubuntu. These limits are critical when running containers or Kubernetes clusters locally, especially under heavier workloads.

So, I’m documenting the necessary changes here to save future me (and maybe you) from having to dig through scattered resources the next time.

---

## Step 1: Increase File Limits with systemd

### Edit `/etc/systemd/user.conf`

``` bash
sudo vi /etc/systemd/user.conf
```

Uncomment and set the following line:

``` ini
DefaultLimitNOFILE=65535
```

### Edit `/etc/systemd/system.conf`

``` bash
sudo vi /etc/systemd/system.conf
```

Again, uncomment and set the following line:

``` ini
DefaultLimitNOFILE=65535:524288
```

---

## Step 2: Tune Kernel Parameters

### Edit `/etc/sysctl.conf`

``` bash
sudo vi /etc/sysctl.conf
```

Add the following lines at the end of the file:

``` ini
net.ipv4.ip_unprivileged_port_start=0
fs.inotify.max_user_instances=2048
```


**Explanation:**

- `net.ipv4.ip_unprivileged_port_start=0`: Allows Kind to bind to ports below 1024 (like 80 and 443), which is useful for ingress testing.

- `fs.inotify.max_user_instances=2048`: Raises the number of inotify watchers available per user. This is needed for Zed and other tools that monitor file changes.


---

## Step 3: Reboot and Verify

Reboot the system to apply the changes:

```bash
 sudo reboot
```


After logging back in, check that the limits have been updated:

```bash

$ ulimit -n
65535

$ sysctl fs.inotify

fs.inotify.max_queued_events = 16384
fs.inotify.max_user_instances = 2048
fs.inotify.max_user_watches = 65536
```


---

## Final Notes

These configuration changes got the new laptop running both Kind and Zed without issue. I suspect they’ll also help ensure the rest of the development environment runs reliably—especially when working with containerized workflows or tools that depend on real-time file system events.

By increasing system limits up front, you can prevent hard-to-diagnose runtime errors and avoid wasting time troubleshooting environment-related problems later on.
