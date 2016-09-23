---
layout: post
title: "Bug detection and prediction at Google"
date: 2011-12-27 13:44:38
image: '/assets/img/'
description:
tags:
categories:
twitter_text:
---
Wow, this was eye-opening! [80,000 builds and 50% of the Google code base changes each month!](http://www.infoq.com/presentations/Development-at-Google) At this scale, their HEAD development (as described in the above link) and  their discipline and detail in unit testing & code reviews is incredible…

[Also, their approach to bug prediction is a very interesting.](http://google-engtools.blogspot.com/2011/12/bug-prediction-at-google.html) Basically they, devised an algorithm that predicts which source files are due to have a bug introduced based on various metrics. This algorithm then pops up a note in their code review tool so that the review can be handed off to a more experienced reviewer.
