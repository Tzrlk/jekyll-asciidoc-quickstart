---
layout: post
title: Why use Markdown for Jekyll sites?
tags:
  - misc
  - meta
date: 2017-05-05T00:00:00.000Z
thumbnail: null
published: false
---

So it turns out that I came up with a reason _not_ to use asciidoc for jekyll sites. While I much prefer asciidoc, it doesn't give me one important feature that markdown does: In-line html.

As much as it annoys me, asciidoc doesn't provide an easy way of adding semantic markup to post content, and therefore I've had to rely on appending a block of json to the end of the article. Fine for defining the data, but less useful for linking that data to specific text.
