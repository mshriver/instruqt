---
slug: cleanup
id: y8mb6lrbgrq7
type: challenge
title: Cleanup
tabs:
- title: Podman
  type: terminal
  hostname: rhel
  cmd: tmux attach-session -t "podman" > /dev/null 2>&1
- title: RHEL Host
  type: terminal
  hostname: rhel
  cmd: tmux attach-session -t "host" > /dev/null 2>&1
difficulty: basic
timelimit: 1
---
Unlike interactive containers, detached containers are stopped using __podman stop <CONTAINER ID>__.

```bash
podman stop $(podman ps -a | grep Up | cut -d" " -f1)
```

In the command above, we use a bit of bash scripting to determine the __CONTAINER ID__ as it is going to be a value unique to each container image.

You can verify that the container is now exited:

```bash
podman ps -a
```

<pre class="file">
CONTAINER ID  IMAGE                         COMMAND               CREATED        STATUS                     PORTS                   NAMES
2b2571efec6f  localhost/rhel9-httpd:latest  /usr/sbin/httpd -...  9 minutes ago  Exited (0) 50 seconds ago  0.0.0.0:8081->80/tcp  priceless_mahavira
</pre>
<a href="#example_image">
 <img alt="An example image" src="../assets/stopped-again.png" />
</a>

<a href="#" class="lightbox" id="example_image">
 <img alt="An example image" src="../assets/stopped-again.png" />
</a>
<style>
.lightbox {
  display: none;
  position: fixed;
  justify-content: center;
  align-items: center;
  z-index: 999;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  padding: 1rem;
  background: rgba(0, 0, 0, 0.8);
}

.lightbox:target {
  display: flex;
}

.lightbox img {
  max-height: 100%;
}
</style>
