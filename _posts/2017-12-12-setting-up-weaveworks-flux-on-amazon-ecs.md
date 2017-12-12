---
layout: post
title: Setting up Weaveworks Flux on Amazon ECS
tags:
  - devops
  - guides
  - docker
categories:
  - devops
date: 2017-12-12T04:17:05.968Z
thumbnail: null
published: 'false'
---
Setting-up a devops continuous delivery pipeline is a feat of epic proportions, and with any luck (I'm writing this as I'm doing it), I'll be able to walk through the steps required to get that pipeline set up with a set of products provided by [Weaveworks][1], namely [Weave Flux][2].

[1]: https://www.weave.works/
[2]: https://www.weave.works/oss/flux/

Setting up the system using [Weave Cloud][3] I'm sure is super-easy, but if you don't have the cash, your POC review period is longer than the 30-day trial, or your infosec department has a hate-on for cloud services, you'll need to install the lot manually.

[3]: https://www.weave.works/product/cloud

* Set up ECS
* Set up k8s
* Install Weave Net
* Install Weave Flux
