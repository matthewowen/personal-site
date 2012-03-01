---
layout: post
title: How this site works
date: 2012-03-01T18:45:00.1Z
---

I promise that I won't keep up a 2:1 meta vs non-meta ratio of posts, but for now it's a relatively fertile field. There are basically three aspects of this site that I'm going to cover: Jekyll, LESS, and Make.

### Jekyll ###

Not being one to shy away from using a currently 'hip' technology, I'm using [Jekyll](https://github.com/mojombo/jekyll). You give it templates and config files, run it on the command line ('jekyll'), and it spits out a static site in a directory - HTML files.

I'm a big fan of this approach. I think a lot of platforms people use for relatively simple applications (like blogging, or a personal website) are enormous overkill, especially because of a tendency for those platforms (such as WordPress or Drupal) to become increasingly generalised over time (there's probably a future blog post in this).

So, the excellent things in Jekyll:

#### Templating ####

In Jekyll, you get the [Liquid templating language](http://liquidmarkup.org/) that probably looks pretty familiar to anyone who's worked with the templates in, say, Django: [here's my 'base' template](https://github.com/matthewowen/personal-site/blob/master/_layouts/base.html). Content is written in [Markdown](http://daringfireball.net/projects/markdown/), which is basically the perfect thing for writing content that's going to become markup.

#### Posts ####

With a platform like Wordpress, when you've produced a blog entry your listing pages are automatically updated too. Excellent. If you were producing a static site by hand, this wouldn't be the case - you'd have to update everywhere. But because Jekyll has a concept of 'posts', you can do [this.](https://gist.github.com/1952010)

When you write a new post for your site, listings around the site are automatically updated. Wonderful.

### LESS ###

To produce CSS, I'm using [LESS](http://lesscss.org/). There's not a lot to say. It's a great way to minimise the amount of CSS you're writing, and to produce CSS files which are a bit more readable. 

### Make ###

I'm not really interested in the way you go about updating dynamic sites. Logging into admin areas, clicking about all over the place, 'saving'. Yuk. With Jekyll, writing a new post just means creating a new file: [see what the file for my last post looked like](https://github.com/matthewowen/personal-site/blob/master/_posts/2012-02-28-the-iron-lady.markdown). This means everything is just version controlled text. This makes me very happy.

Once I've written a new post, it's easy to go live. At the shell, I just type 'jekyll --server', which generates the static site and starts a server to preview it. Alongside that, I've got a Makefile (inspired by [this post](http://www.andreimackenzie.com/Jekyll/)). It looks something like this:

	publish:
		rsync -avz --exclude Makefile _site/ username@servername:/home/public

If I'm happy with how the post is looking, all I have to do is type 'make' at the shell and enter my password. Then there we are - the static directory gets rsynced up to my server and the new post is live. Having easy deployment that can also be rolled back is a really great thing. Two commands and one password entry is a pretty pain free system.