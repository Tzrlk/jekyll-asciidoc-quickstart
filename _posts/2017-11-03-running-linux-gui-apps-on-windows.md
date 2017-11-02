---
layout: post
title: Running Linux GUI apps on Windows
tags:
  - docker
  - windows
  - gui
categories: null
date: 2017-11-02T22:42:54.488Z
thumbnail: null
published: true
---
I just managed to get a Linux GUI app running on my windows desktop without the need for a virtual machine. This must seem old-hat to Linux pros, but damn it opens up a world of possibilities.

Thanks to [Jessie Frazelle's blog post on running docker containers with guis][1], and [qinwf's github comment on a moby repo issue][2], I was able to get linux firefox running from a container.

The steps are pretty simple:

1. Install [XMing Server][3] (I did so [via chocolatey][4])
2. Run 'XLaunch' and disable access control.
3. Run your docker image with the environment variable "DISPLAY" pointing to your hostname and display number. e.g

        docker run -e DISPLAY=myhostname:0.0 jess/firefox

Note: The `:0.0` is just pointing to whatever display has been set up, you can check this in the logs as near the end of the startup sequence, it'll spit out something like

    winClipboardProc - DISPLAY=127.0.0.1:0.0

If that number at the end is different, use that one, I suppose.

While my example has been done with Docker, doing exactly the same for a linux desktop should be no problem either. You could even just invoke application startup via ssh commands, or forward your whole x-windows content, I suppose. I'll have to try that next.

[1]: https://blog.jessfraz.com/post/docker-containers-on-the-desktop/
[2]: https://github.com/moby/moby/issues/8710#issuecomment-135109677
[3]: https://sourceforge.net/projects/xming/
[4]: https://chocolatey.org/packages/xming

