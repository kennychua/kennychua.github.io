---
layout: post
title: "Exploration into Twitter bots and cryptocurrencies"
date: 2018-05-15 15:24:36
image: '/assets/img/'
description:
tags:
categories:
twitter_text:
---

I give this plugin two :+1:!


hello <sup> world </sup> <sub> <a href="https://www.google.com">is possible.i</a></sub>
## Title : Exploration into Twitter bots and cryptocurrency

### Hypothesis/What If
In cryptocurrency trading, there is huge potential upside as a trader if you are one of the quickest to trade when a new coin/cryptocurrency is added to an exchange

Specifically, there are at leasts to 2 scenarios
#### First exchange listing for an ICO

#### Established cryptocurrency being added to a large exchange


most exhcanges are API first, what if we could watch the APIs

furthermore, could it be that new cryptocurrencies are soft-launched via the APIs first before the web interfaces actually display the currency?

Turns out both of those what ifs are true!

Let's get to work...

### Prior knowledge before exploration
No prior knowlege in this domain

### Challenges
#### Challenge 1 : 
while most of exchanges offer apis and enough functionality exposed via the APIs for the purpose of scraping information for new coin listings, it turns out that there there is no standard for theAPIS

    need to write custom logic to interact with each exchange's API, eg with authentication and signing each reqyest

    After some looking, looks like there is a really nice abstraction layer - ccxt (link to github)


#### Challenge 2 : twitter api

#### Challenge 3 : learn hosting
initially lambda function, but want to potnetially run with frequency < 1 second so rules out schedules on lambda function
also need storage, so decide to spin up a cheap Virtual Private Server - looked at DigitalOcean, Linode and Vultr, evenruallt settled on Vultr


#### Challenge 4 : try async/await syntax
aws lambda don't support yet unless via babel transpilation
some inline `code`


:wq
