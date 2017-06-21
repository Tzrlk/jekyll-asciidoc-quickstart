---
layout: post
title: Updating Git on Centos 6/7
tags:
  - sysops
  - linux
  - centos
categories:
  - tech
  - sysops
date: 2017-05-22T00:00:00.000Z
thumbnail: null
published: true
---

So, I had to update git on a couple of Centos machines last week, and I ran into what appears to be a very common problem: Centos only has old versions of stuff in its repos. Subsequently, I had to figure out how to get the latest, and preferably from a yum repo, rather than installing it manually. Cue repo connection issues.
{: itemprop="description" }

## The Story

When working behind a corporate proxy, a number of things can go awry without any good indication of what is _actually_ wrong. Installing the needed repos gave me no issues on one server (centos 6), but threw a fit trying to connect on another. The reason? SSL validation failure. The solution? Depends on how much you want to compromise your system.

A bunch of people suggested just changing the repo url in your /etc/yum.repos.d/epel to be http instead of https, but that's dumb. Another suggested updating the ca-certs package, but that still won't work if you had the same issue I did: Self-signed root CA certificates with an intercepting proxy.

## The Method

The quick guide to installing the latest git on Centos is fairly straightforward:

{% highlight shell %}

	# Update certs package:
	yum update ca-certificates curl nss

	# Install Company CA Certificate:
	update-ca-trust force-enable
	cp ${certfile} /etc/pki/ca-trust/source/anchors/
	update-ca-trust extract

	# Install IUS repo:
	rpm -i https://centos${version}.iuscommunity.org/ius-release.rpm || {

		# If there's trouble installing the epel releases repo:
		rpm -i https://dl.fedoraproject.org/pub/epel/epel-release-latest-${version}.noarch.rpm

		# Then just repeat the ius install.
		rpm -i https://centos${version}.iuscommunity.org/ius-release.rpm

	}

	# Update yum cache and install git:
	yum makecache fast
	yum install git2u -y || {

		# If there's trouble installing the git package:
		yum clean all

		# Then just repeat the install.
		yum makecache fast
		yum install git2u -y
	
	}

	# You may need to log-out and log-in again to get the shell to refresh your git
	#     version, but after that, everything should be working just fine.
	
{% endhighlight %}

## The Conclusion

Thankfully, you'll only need to ever do the first part once. Steps 4-5 may be necessary for other packages however. It's a pain that it doesn't tell you that the issue is an SSL cert trust failure, but googling the error turns up plenty of help. Hopefully this one will get some notice.

## Update!

Apparently, something has happened recently that caused even this method to not work properly. After some grey hairs and crying, I figured-out that the `nss` package needed updating as well as `curl`. Once that was complete, everything worked fine again. I've updated the script accordingly.
