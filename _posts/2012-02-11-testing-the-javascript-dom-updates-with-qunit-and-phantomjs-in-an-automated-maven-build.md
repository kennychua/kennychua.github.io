---
layout: post
title: "Testing the JavaScript DOM updates with QUnit and PhantomJS in an automated Maven build"
date: 2012-02-11 13:46:02
image: '/assets/img/'
description:
tags:
categories:
twitter_text:
---
Greetings all,

Now that we’ve been though the details of how to [set up a simple assertion test with QUnit and PhantomJs in a Maven Continuous Integration build in part 1](https://kennychua.github.io/running-qunit-javascript-tests-in-a-maven-continuous-integration-build-with-phantomjs/), let’s take a closer look at all the assertions that are available in a test.

The techniques here are dependent on some libraries, but this should already be included if you are using the [phantomjs-qunit-runner maven plugin](http://code.google.com/p/phantomjs-qunit-runner/). Credit to [this article](http://indomitablehef.com/?cat=35) to for the DOM setup and teardown helper functions.

Alright, onward to the code examples!

Suppose we had a javascript function that does a simple DOM manipulation to add some text to a given element as such.

{% highlight JavaScript %}
JSDEMO.quickstart = function () {
 
    /**
     * Public interface
     * @scope JSDEMO.quickstart
     */
    return {
 
        /**
         * Appends some text to an input element ...
         * @param addToElement Element to append some text to
         */
        appendChildNodeToDom : function(addToElement) {
            var newnode = document.createTextNode('Here is some content.');
            document.getElementById(addToElement).appendChild(newnode);
        }
    };
} ();
{% endhighlight %}

Here is the code to test it! Pretty simple really.. comments inline

{% highlight JavaScript %}
test("Can append child to element in DOM", function() {
    // expected results for the test to compare against
    var expectedTestDivHtml = "Here is some content.";
 
    // Helper function to set up DOM element to test with
    DOMSetup('<div id="testDiv"/>');
 
    // Call our function to be tested with the DOM element created by the helper above
    JSDEMO.quickstart.appendChildNodeToDom('testDiv');
 
    // Test! See if results is as expected
    equal(expectedTestDivHtml, $('#testDiv').html());
 
    // Helper function to tear down DOM element tested with
    DOMTearDown('#testDiv');
});
{% endhighlight %}

