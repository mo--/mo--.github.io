---
layout: post
title:  "Fix Java 'trustAnchors parameter must be non-empty' error on Alpine Linux"
date:   2015-04-01 11:59:48
categories: alpine java
---

The easiest way to fix the java error `the trustAnchors parameter must be non-empty`'on Alpine Linux, is to copy the `/etc/ssl/certs/java/cacerts` file form another distro -eg. Arch Linux- to `/usr/lib/jvm/java-1.7-openjdk/jre/lib/security/`

That's all :)
