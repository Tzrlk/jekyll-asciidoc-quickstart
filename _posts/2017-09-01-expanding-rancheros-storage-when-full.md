---
layout: post
title: Expanding RancherOS storage when full
tags:
  - vmware
  - rancheros
  - devops
  - sysops
categories:
  - devops
  - rancheros
date: 2017-09-01T02:45:41.633Z
thumbnail: null
published: true
---
I ran into some trouble with my rancheros vms and the disk space they had allocated to them. This is how I solved that problem.

## Steps

1. Delete all snapshots of the machine.

2. Increase the disk size.

3. Take a snapshot in case something goes wrong.

3. Connect via remote console and mount a gparted cd.

4. Expand the partition to fill the available space.

5. Reboot and make sure you update the config with:

   ```yaml
   rancher:
     resize_device: /dev/sda
   ```

   to avoid needing to do steps 3-6 again.

Once you've done all that, you should be fine.
