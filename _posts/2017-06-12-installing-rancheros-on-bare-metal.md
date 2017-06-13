---
layout: post
title: Installing RancherOS on Bare Metal
tags:
  - rancher
  - docker
  - sysops
  - devops
categories:
  - tech
  - devops
date: 2017-06-12T02:55:24.133Z
thumbnail: null
published: true
---

Getting RancherOS installed on bare metal is a pain if you don't know all the steps. I had to figure these out from the various bits of documentation scattered-around the place. Docker + corporate proxy = a pain in the ass.

## Setup

0. Download the iso
: Just grab the latest iso from the [RancherOS github releases](https://github.com/rancher/os/releases).

0. Install it to bootable media
: This can be done via a number of means. I originally ran [Rufus](https://rufus.akeo.ie/) to do the job
: With subsequent installs I'll be going with `dd if=rancheros.iso of=/dev/sdx` (where `sdx` is whatever the usb device is).
: Apparently [Clonezilla](http://clonezilla.org/) is also an option, but the simplicity of `dd` makes it pretty damn appealing.

0. Boot the OS
: Just make sure that the system will accept booting from the usb drive. This may require turning-off secure boot and using the legacy features.

0. Change the rancher password
: Just run `sudo passwd rancher`, followed by something like `password` and `password`. Ignore the errors, it should work just fine regardless. Once done, you'll be able to ssh and scp into the box just fine.

0. Set up the cloud-config.yml file
: Put whatever config you need in a cloud-config.yml file, and get it onto the box via ssh or scp.
: You can do all the configuration via `sudo ros config set`, but that can become a pain _really_ quickly, so it's just easier to write it once and use `sudo ros config validate -i cloud-config.yml` and `sudo ros config merge -i cloud-config.yml` to apply it. I'll put a sample config below.

0. Wipe the hd partitions
: This is fairly easy, as a simple `sudo fdisk /dev/sda <enter> d <enter> w`, should wipe the partition table for the primary HD, making it ready for install.

0. Pull the latest os installer image
: Assuming you've updated the relevant network settings, you can make sure they're applied by running `sudo system-docker restart docker`, `sudo system-docker restart network`, then running `docker pull rancher/os:vX.X.X` and/or `sudo system-docker pull rancher/os:vX.X.X`.
: Alternately `docker save` and `docker load` 
 combined with `scp` can be used to bypass `docker pull` if that's being a pain.

0. Run the ros installer on the hd
: The final step, you should have a `cloud-config.yml` available, as well as the `rancher/os:vX.X.X` image pulled to the repo. Now you just need to run `sudo ros install -i rancher/os:vX.X.X -c cloud-config.yml -d /dev/sda` and wait for it to finish and reboot.

## Config Example

This is an obfuscated version of my current working config, so some of it may or may not be necessary for your purposes (or even at all). As far as I know, there's no variable substitution, so replace all the `${...}` instances with actual values (hence `PROXY_HTTP` instead of `HTTP_PROXY`).

```yaml
rancher:

  environment:
    - EXTRA_CMDLINE=/init
    - HTTP_PROXY=${PROXY_HTTP}
    - HTTPS_PROXY=${PROXY_HTTPS}
    - NO_PROXY=${PROXY_EXCLUDE}

  network:
    dns:
      nameservers:
        - ${DNS_IP_1}
        - ${DNS_IP_2}
      search:
        - ${COMPANYDOMAIN_LOCAL}
    interfaces:
      eth0:
        dhcp:    true
        gateway: ${GATEWAY_IP}
    http_proxy:  ${PROXY_HTTP}
    https_proxy: ${PROXY_HTTPS}
    no_proxy:    ${PROXY_EXCLUDE}

  services:

    # If you're using convoy
    convoy:
      name:  convoy
      image: evansgp/convoy-daemon:latest
      command:
        - daemon
        - --drivers     devicemapper
        - --driver-opts dm.datadev=/dev/sdb1
        - --driver-opts dm.metadatadev=/dev/sdb2
      privileged: true
      restart:    always
      labels:
        io.rancher.os.scope: system
        io.rancher.os.after: docker
      volumes:
        - /var/run/docker.sock:/var/run/docker.sock
        - /etc/docker/plugins:/etc/docker/plugins
        - /dev/sdb1:/dev/sdb1
        - /dev/sdb2:/dev/sdb2
      volumes_from:
        - all-volumes

    rancher-agent:
      name:        rancher-agent
      image:       rancher/agent:v1.2.2
      command:     http://${RANCHER_HOST_PORT}/v1/scripts/${RANCHER_CODE}
      privileged:  true
      autodestroy: always
      volumes:
        - /var/run/docker.sock:/var/run/docker.sock
      labels:
        io.rancher.os.after: docker

  services_include:
    rancher-server: true

  system_docker:
    environment:
    - HTTP_PROXY=${PROXY_HTTP}
    - HTTPS_PROXY=${PROXY_HTTPS}
    - NO_PROXY=${PROXY_EXCLUDE}

write_files:

  - path: /opt/rancher/bin/start.sh
    permissions: "0755"
    owner: root
    content: |
      #!/bin/bash
      alias convoy='sudo system-docker exec -it --privileged convoy convoy'

ssh_authorized_keys:
  - ${PUBLIC_KEY_1}
  - ${PUBLIC_KEY_2} # etc.

```
