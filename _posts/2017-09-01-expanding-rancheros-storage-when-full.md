---
layout: post
title: Expanding RancherOS storage when full
tags: null
categories:
  - devops
  - rancheros
date: 2017-09-01T02:45:41.633Z
thumbnail: null
published: false
---

## Steps

1. Delete all snapshots of the machine
2. Increase the disk size
3. Connect via remote console and mount your rancheros iso
4. Run `sudo e2fsck -f /dev/sda1`
4. Run `sudo resize2fs /dev/sda1`
