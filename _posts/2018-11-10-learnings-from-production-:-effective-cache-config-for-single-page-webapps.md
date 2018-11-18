---
layout: post
title: "Lessons from Production : Effective Cache Config for Single Page Webapps"
date: 2018-11-10 22:55:45
image: '/assets/img/'
description:
tags:
categories:
twitter_text:
share: linkedin twitter
---

# Lessons from Production - Effective Cache Config for Single Page Webapps



Welcome to the first of a series of posts about general knowledge from running Single Page App at scale in Production!



In this edition, we discuss the best practices for caching resources in a Single Page App - Angular/AngularJs/React/Vue or similar - to achieve a balance between performance, flexibility and not shooting yourself in the foot.



First up, let's assume your app that has a build process (webpack or otherwise) to produce the typical `*.js`, `*.css`, `*.html` resources.



![Alt text](https://raw.githubusercontent.com/kennychua/kennychua.github.io/master/assets/img/caching_file_layout.png)


Once the build process is complete, end up with 2 classes of resources,

- ***'Immutable Resources'*** with a filename unique to this build, eg `main.8d7a94ef1418343a7897.js`,
- ***'Mutable Resources'*** with a constant filename across builds, eg `index.html`

## TLDR; 
- For ***Immutable Resources***, make sure your server responds with the following HTTP Headers : 
```
Cache-Control: public, max-age=31536000
```
- This instructs browsers (and all the proxies that played a role in delivering this content to the browser) to use a cached copy where possible for the next 1 year. This is perfect, since this file's name was generated with a hash of the file contents, and we know the file name for this copy of the file will never change

![immutable resources caching](https://raw.githubusercontent.com/kennychua/kennychua.github.io/master/assets/img/caching%20-%20Immutable%20Resources.png)

--------------

- For ***Mutable Resources***, make sure your server responds with the following HTTP Headers : 

```
Cache-Control: max-age=30
ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"
```
- By ensuring that an ETag is returned, this means that the browser can issue a conditional `GET` request in subsequent requests to get a new copy of the file **only if** the file has been updated on your server.
- Notice that we also tell the browser to cache for 30 seconds. In case we introduce a bug that causes the page to continually refresh, we avoid overwhelming our servers with a ton of requests

![mutable resources caching](https://raw.githubusercontent.com/kennychua/kennychua.github.io/master/assets/img/caching%20-%20Mutable%20Resources.png)

------------------

## The details
### What about using `Cache-Control : immutable`?

This new cache control extension value may be tempting to use, but prefer to use more traditional values to achieve more expected behaviour across browsers. 
Certain browsers either don't understand, or choose to ignore the `immutable` directive
- https://stackoverflow.com/questions/41936772/which-browsers-support-cache-control-immutable
- https://stackoverflow.com/questions/52762478/reasons-why-google-chrome-is-ignoring-cache-control-header

------------

### Why not use `Cache-Control: no-store, must-revalidate`?
Using `no-store, must-revalidate` may be useful in scenarios where we want to strictly **not** cache any content - especially sensitive content.

However, since our application is a SPA consisting of `*.js`, `*.css`, `*.html` resources which by definition cannot be sensitive, we prefer that the browser cache where possible to optimise for quicker loading

-------------

### Why `Cache-Control: max-age=30` ?
In the off chance that we accidentally introduce a bug that results in our app continually refreshing, we protect our servers from being overwhelmed by telling the browser to cache resources for up to 30 seconds.

After a number of refresh loops, most browsers should detect the fault and prevent the looping from continuing forever

-------------



*Note : Opinions are my own and do not represent the views or state of my employer*


