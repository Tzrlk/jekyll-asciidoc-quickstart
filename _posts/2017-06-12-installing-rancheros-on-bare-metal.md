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
---
* Download the iso
* Install it to bootable media
* Boot the os
* Set up the network settings
* Reload the network
* Change the rancher password
* SSH into the box
* Set up the cloud-config.yml file
* Wipe the hd partitions
* Pull the latest os installer image
* Scp the result of docker save to the box
* Docker load the image into system-docker
* Run the ros installer on the hd
