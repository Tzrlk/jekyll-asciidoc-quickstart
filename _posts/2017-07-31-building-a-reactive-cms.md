---
layout: post
title: Building a Reactive CMS
tags:
  - angular
  - reactive
  - cms
categories:
  - design
  - web
  - cms
date: 2017-07-30T21:45:49.498Z
thumbnail: null
published: true
---
I've had an idea rolling around in my head for a while, of a fully reactive JIT-compiled CMS written in Angular, and running on something that isn't PHP. Every now and then I have an ephiphany which shapes this idea further.

Now, knowing my luck, by the time I've actually fully designed this thing, it'll not only be out of date, but it'll have already been implemented in some half-assed way with PHP. This won't stop me dreaming, however.

The thought I just had revolves around the transport mechanisms of getting session data to the client, while still utilising the native caching mechanisms of HTTP et. al. to avoid re-inventing the wheel. Previously, I had thought that doing everything over websocket was the best way, but now I realise that websocket is far better for use as a signalling channel, and any actual data should be fetched via http.

To provide some context, part of my idea is to use HTML local/session storage to maintain all the data needed for the scripts to render any and all components displayed on whatever pages are accessed. Http with the use of promises allows lazily-loading data and a reasonable way of displaying load progress.

Of course, there's still the question of encrypting certain data on the users' local storage for later use, just how to organise and synchronise the session dataset, and which js framework to use if any. All of that being just a lead-up to the actual CMS portion of the system which needs to do all the annoying drag-and-drop that makes it useable, and whatever to do about the back-end.

Still a lot to work on, but I'm just glad things are still crystalising to a structure in my mind that doesn't make me want to vomit in my mouth.
