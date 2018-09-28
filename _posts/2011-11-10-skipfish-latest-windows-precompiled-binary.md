---
layout: post
title: "Skipfish Latest Windows Precompiled Binary"
date: 2011-11-10 13:43:44
image: '/assets/img/'
description:
tags:
categories:
twitter_text:
---

Skipfish is a great and FAST automated pen test tool with a low barrier to entry by Google's own web security expert Michal Zalewski.

Skipfish is distributed as source only at [http://code.google.com/p/skipfish/](http://code.google.com/p/skipfish/) and I've looked around the web for a compiled Windows version but couldn't find one so I'd thought I would share instructions on how to build Skipfish on Windows in Cygwin.

I hope to provide a Windows executable that I will keep up to date as releases roll out.

How to Compile Skipfish On Cygwin
--------------------------------

1. Install Cygwin – www.cygwin.com
2. Go through standard the Install Wizard steps until the ‘Cygwin Setup – Select Packages’ screen. Select the following additional packages & install the following. ‘devel’ versions if available
    1. All -> Base -> zlib
    2. All -> Devel -> openssl-devel
    3. All -> Devel -> libiconv
    4. All -> Devel -> make
    5. All -> Devel -> gcc
    6. All -> Devel -> pcre
    7. All -> Libs -> libiconv2
    8. All -> Libs -> libidn
3. Download the Skipfish code from http://code.google.com/p/skipfish/
4. Open up cygwin, and extract the code
5. Run make to generate the executable
    1. make
    2. make install
6. Voila, you have your executable

And here's one I prepared earlier! [Skipfish V2.03 Windows Pre-Compiled Binary]({{ site.url }}/assets/others/skipfish-2.03b.zip)
