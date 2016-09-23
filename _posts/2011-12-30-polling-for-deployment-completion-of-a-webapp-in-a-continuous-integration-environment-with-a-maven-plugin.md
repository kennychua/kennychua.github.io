---
layout: post
title: "Polling for Deployment Completion of a Webapp in a Continuous Integration Environment with a Maven Plugin"
date: 2011-12-30 13:45:08
image: '/assets/img/'
description:
tags:
categories:
twitter_text:
---

The typical workflow of a Continous Integration/Continuous Deployment process is fairly well understood. As a quick recap, code is checked out regularly, then built, and following that a whole suite of tests are run to confirm that the code changes did in fact fulfill its purpose and did not unintentially cause other parts of the app to misbehave.

In web applications, it is becoming increasingly popular (as it should be) to have the application deployed to a container (eg, using Cargo) so that automated UI tests can be run via Selenium or Watir or QTP or what have you.


Problem
---------------------
The deployment step is often asynchronous (ie. it does not wait until complete before it returns). Since your automated UI tests should only run once your app is deployed to the container and has started, how do you ensure that is the case?

Solution
---------------------
Enter the maven-urlpoller-plugin.

This Maven plugin is designed to be used in a continuous deployment scenario where the command to deploy an application to an app server is asynchronous.

This Maven plugin will poll a given url for a specified period (while blocking) until it times out, or gets an expected response (yay, deploy complete!).

Usage
---------------------
Include the following in your Maven pom.xml configuration. See http://code.google.com/p/maven-urlpoller-plugin/ for detailed configuration documentation.
{% highlight XML %}
<plugin>
    <groupId>net.kennychua</groupId>
    <artifactId>maven-urlpoller-plugin</artifactId>
    <configuration>
       <pollUrl>http://www.google.com</pollUrl>
       <statusCode>200</statusCode>
       <secondsBetweenPolls>10</secondsBetweenPolls>
       <repeatFor>5</repeatFor>
       <failOnFailure>false</failOnFailures>
    </configuration>
</plugin>
{% endhighlight %}


In this example, it will poll www.google.com for the HTTP status code 200 (OK). If it does not receive the expected response, it will wait for 10 seconds and then try again up to 5 times in total.

As a handy tip, when several plugins are configured to the same phase in a maven lifecycle, they are executed in the order in which they are declared in the pom.xml. This is true in Maven 3.0.3 and later.
