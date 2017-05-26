---
title: Fixing Docker Swarm state on Windows
tags: docker docker-swarm devops windows
---

So, after smashing my head against the desk for a good few hours, I finally figured-out how to clear a 'pending' docker swarm state, and get things back-on-track.

Usually, you'd just run `docker swarm leave` or even adding `--force`, but in this case, the only response received is `Error response from daemon: context deadline exceeded`. Not very helpful.

Adding `--debug` and `--log-level=debug` to the invocation doesn't provide any more useful information, either. Restarting the service doesn't do anything, and the internet isn't too helpful (hence this post).

After reading [This article][ref1], I noticed something very significant:

> As a temporary workaround I manually found the location of docker inside the VM by mounting various bits of its filesystem as a volume in a container and then ls-ing, and I've hardcoded that location in my scripts, but that's not a great solution.

Realising that I might be able to actually mount the file system of the moby vm, I tried my luck with `docker run -it --rm -v /var/lib/docker/:/var/lib/docker/ alpine:3.5 sh`. Miracles of miracles it worked flawlessly, so I followed that up with `rm -rf swarm`, exited the container, and restarted the service. A subsequent call to `docker info` showed me exactly what I wanted: no mention of docker swarm.

Now I can get on with peeling the next few layers of the onion, and maybe I'll be able to actually get something working. My plan is to install a local [Rancher][ref2] instance and use it to admin other machines via ssh. Ranting about docker tooling is for another day.

[ref1]: https://forums.docker.com/t/how-can-i-ssh-into-the-betas-mobylinuxvm/10991
[ref2]: http://rancher.com/
