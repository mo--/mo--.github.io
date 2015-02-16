---
layout: post
title:  "Funtoo package management"
date:   2015-02-15 11:59:48
categories: funtoo package
---

## Introduction

Funtoo is really a great linux distribution, and is my new choice for those reasons:

- Compilation from source: it's more long than using binary packages but you can really feet you installation to your needs, not more, not less
- Gentoo based: like Gentoo but with a better portage tree
- Not forced to use Systemd: Systemd could be a good choice, but imho the project is immature, also it does thinks I don't expect for an init system

I guess there's other reasons to switch to Funtoo, realtime, the community, etc.

This post aim to create a minimal survival guide for packages management

## Install and uninstall with emerge

`emerge` is the main application to "emerge" -download, unpack, patch,  compile, install and merge- [ebuild][ebuild] -the package script-, you can find information on the basic commands and configuration files management on [funtoo emerge wiki][emergewiki]. More if you RTFM indeed

## Query your installed packages with equery

`emerge` is a great tools but doesn't give you informations about installed packages, this is the `equery` purpose, informations about basic usage on [gentoo equery wiki][equerywiki].

## Search with eix

`emerge` has a search functionality, but it's really slow and lack of efficiently and flexibility, that's why `eix` is for, information about usage on [gentoo eix wiki][eixwiki]

## Managing overlays with layman

Overlays are alternative package repositories, you could find many things is other overlays that are not in the main one called the *Portage Tree*.

If you plan to install something from an other overlay use `layman` to fetch, add, remove and update them, all information is available on [gentoo layman wiki][laymanwiki]

## Manage your patches with foobashrc

Sometimes you want to create a patch for a package, for example [st][st] configuration file is [config.h][config.h] so the better way to configure it, is to create a patch for `st/config.h`, there's many other example where you can need patching applications.

This is exactly the `foobashrc`'s purpose, it's really easy to use and so powerful, you don't need to re-write the ebuild, it's so cool, all information is available on [funtoo foobashrc wiki][foobashrcwiki].

to be removed

[ebuild]: http://wiki.gentoo.org/wiki/Basic_guide_to_write_Gentoo_Ebuilds
[emergewiki]: http://www.funtoo.org/Emerge
[equerywiki]: http://wiki.gentoo.org/wiki/Equery
[eixwiki]: http://wiki.gentoo.org/wiki/Eix
[laymanwiki]: http://wiki.gentoo.org/wiki/Layman
[st]: http://st.suckless.org/
[config.h]: http://git.suckless.org/st/tree/config.def.h
[foobashrcwiki]: http://www.funtoo.org/Applying_Local_Patches_to_Ebuilds
