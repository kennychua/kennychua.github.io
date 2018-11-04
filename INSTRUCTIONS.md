## Creating posts

You can use the `initpost.sh` to create your new posts. Just follow the command:

```
./initpost.sh -c Post Title
```

The new file will be created at `_posts` with this format `date-title.md`.

## Front-matter 

When you create a new post, you need to fill the post information in the front-matter, follow this example:

```
---
layout: post
title: "How to use"
date: 2015-08-03 03:32:44
image: '/assets/img/post-image.png'
description: 'First steps to use this template'
tags:
- jekyll 
- template 
categories:
- I love Jekyll
twitter_text: 'How to install and use this template'
---
```

## Running the blog in local

In order to compile the assets and run Jekyll on local you need to follow those steps:
- `sudo gem install jekyll`
- `gem install bundler`
- `bundle install`
- Install [NodeJS](https://nodejs.org/)
- Run `npm install` 
- Run `gulp`



## Cheatsheet
https://jekyllrb.com/docs/posts/
https://jekyllrb.com/docs/templates/

## inserting image
![Basic CI Workflow]({{ site.url }}/assets/img/CI_diagram.png)

header
inserting code. list of types available at: http://rouge.jneen.net/
{% highlight XML/shell/Java/JavaScript %}
code
{% endhighlight %}

inserting blockquote
> this stuff will be blockquoted. end with an emptyline

this stuff will not be blockquoted


# Cross post to medium
- https://github.com/jondot/mediumize
```
$ gem install mediumize
$ mediumize -t your-medium-integration-token file1.md file2.md ... fileN.md

```
