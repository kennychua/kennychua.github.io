---
title: Running QUnit JavaScript tests in a Maven Continuous Integration Build with
  PhantomJs
date: 2011-12-14 13:44:14 Z
layout: post
image: "/assets/img/"
description: 
twitter_text: 
---

Problem :
-----------
QUnit tests are great, but they only run in my browser locally. I want to be able to run these tests locally while developing yet still be able to hand those tests off for automated testing by Maven when my build is run..

When presented with this problem several weeks ago, naturally I turned to the web for existing solutions but could not find one! The closest I came to finding a published solution was at http://whileonefork.blogspot.com/2011/07/javascript-unit-tests-with-qunit-ant.html where the writer talks about an Ant solution.

What to do? Write a Maven plugin and share, of course!

Solution outline :
-----------
QUnit needs to run in a browser. Instead of choosing something like RhinoJs which provides only a JavasScript engine, I selected PhantomJs instead. PhantomJS is a headless WebKit with JavaScript API. It has fast and native support for various web standards: DOM handling, CSS selector, JSON, Canvas, and SVG.

I then needed to capture the output of the test results on QUnit was finished running. Luckily, the QUnit framework provided handy callback functions that made this easy. Also, props to Rod for getting me started.

What was left was only to write the Maven plugin which ties the source JavaScript file to be tested, the JavaScript/QUnit test file and PhantomJs altogether so the Maven lifecycle magic could happen!

How to use:
-----------
Include the following in your Maven pom.xml configuration. See http://code.google.com/p/phantomjs-qunit-runner/ for detailed configuration documentation.

{% highlight XML %}
<plugin>
    <groupId>net.kennychua</groupId>
    <artifactId>phantomjs-qunit-runner</artifactId>
    <configuration>
      <jsSourceDirectory>../src/main/js/..</jsSourceDirectory>
      <jsTestDirectory>../src/test/js/..</jsTestDirectory>
      <ignoreFailures>false</ignoreFailures>
      <phantomJsExec>../path/to/phantomjsexec/..</phantomJsExec>
    </configuration>
</plugin>
{% endhighlight %}

Then, place your JavaScript file to be tested (hereafter referred to as JavaScript source) in a location in your Maven project, in this example it is src/main/js.

Next, place your JavaScript/QUnit test files (hereafter referred to as JavaScript tests) in another location in your Maven project as per Maven convention. In this example, it is src/test/js.

![Folder Structure]({{ site.url }}/assets/img/js_mvn_folderstructure.png)

This plugin works by convention. Given a JavaScript test file called FooTest.js, the plugin will run PhantomJs, load up QUnit, load up Foo.js, then run the defined tests. The results are then handled accordingly.

![Source]({{ site.url }}/assets/img/js_mvn_foobarjs.png)
![Test]({{ site.url }}/assets/img/js_mvn_fooBarTest.png)

Finally, let’s see the magic happen! The plugin is tied to the Maven test lifecycle.

{% highlight shell %}
mvn test
{% endhighlight %}

produces the following result

![Result]({{ site.url }}/assets/img/Screenshot-at-2011-12-04-221954.png)

Success! .. well.. it’s a build failure, but that is the point as I deliberately had a test failure

 

Another nice feature is that it generates the HTML QUnit wrappers you would normally use when using QUnit. This should make development work even easier by being able to run your tests locally in your browser. Look in the Maven target/qunit-html folder.



 

Next step : Make the test output JUnit xml compliant (currently untested), make the option to load up other JavaScript files (eg containing helper functions definitions used in your JavaScript) before running the tests.

[Update: How to test JavaScript DOM updates with this plugin!](https://kennychua.github.io/testing-the-javascript-dom-updates-with-qunit-and-phantomjs-in-an-automated-maven-build/)
