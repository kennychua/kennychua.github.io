---
title: LinkedIn’s Continuous Integration for Mobile
date: 2011-12-27 13:44:48 Z
layout: post
image: "/assets/img/"
description: 
twitter_text: 
---

LinkedIn has an [interesting approach to Continuous Integration](http://engineering.linkedin.com/testing/continuous-integration-mobile) in their mobile app. Of special interest are the ‘Fixture tests’ which mock HTTP/AJAX requests, and the ‘Layout tests’ which does a visual comparison against baseline! Would be very interesting to try and integrate the Layout tests into my automated builds..

> Our mobile CI consists of a five stage pipeline:
1. Mobile continuous integration pipeline
2. Unit tests: run in less than 10 seconds to test individual modules/units
3. Fixtures tests: test solely the client apps by having them use static/mock data
4. Layout tests: test the appearance of the client apps by taking screenshots and comparing them against baseline images
5. Deployment: automatically deploy to a staging environment
6. End-to-end tests: run end-to-end tests against the staging environment

I’d suggest following the [LinkedIn Engineering blog](https://engineering.linkedin.com/blog) as they often have very interesting articles about their approaches to solving problems.
