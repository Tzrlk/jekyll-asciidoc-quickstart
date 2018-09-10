---
layout: post
title: Browsing a Docker container filesystem
tags:
  - docker
categories:
  - devops
date: 2018-09-10T23:33:49.562Z
thumbnail: null
published: true
---
For a while, there’s been an itch when dealing with docker containers of any kind that the default tooling hasn’t been able to scratch yet. There are an assortment of half-baked tools for dealing with various docker functions, many of which get incorporated into base Docker functionality, but none that adequately deal with the task of browsing the file system of a container.

Part of the problem seems to be that there’s no bridge available that allows existing GUI browsers to access the filesystems of containers. There’s no command that’ll start an ssh/scp server for a given container, or provide some other virtual interface beyond having to shell into the container and browse things from there.

There are a few attempts that caught my notice during my latest attempt at googling for an answer, and while I haven’t tried any of them yet, they provide some interesting approaches I might try.

* [Docker SSH][1] - A straight-up SSH bridge to docker containers, as easy to set-up as running a single command. Sadly, SCP/SFTP support is still on the roadmap, and considering what I understand of the underlying mechanism of this project, unlikely to be available any time soon.
* [devicemapper/overlay on host][2] - Docker has to store the filesystem data somewhere during use, and if you can get into the underlying storage, you can browse the files from there. However, which version of docker you're running will affect the method and location of storage, and if you're running on Windows, that means [access to the moby vm][3].
* [procfs access][4] - Nearly identical to the previous entry, accessing the files under the procfs for the cgroup the container is running under is a potential option.

So unless docker implements a virtual filesystem, or someone releases a tool to do so, we'll be stuck with `docker exec` and hacking solutions together with bailing twine and tears.

I'll update if I find something useful.

[1]: https://hub.docker.com/r/jeroenpeeters/docker-ssh/
[2]: https://stackoverflow.com/a/27196037/41223
[3]: https://blog.jongallant.com/2017/11/ssh-into-docker-vm-windows/
[4]: https://superuser.com/a/1288058/307322

