---
layout: post
title: Creating my own website
author: Brendan Thompson
date:   2017-08-15 2:30:00 -0400
permalink: /website-design
categories: blog
tags: Web-Design
excerpt: Check out this brief description of getting started with Jekyll and getting my first website off the ground
github: https://github.com/brenthompson2/brenthompson2.github.io

image: /assets/img/post-images/website-header-pic.png
imageAlt: Jekyll Logo
image-slider: /assets/img/post-images/slider-images/website-header-pic-slider.png
---

To create this website I used [Jekyll](https://jekyllrb.com) and hosted it using [GitHub Pages](https://pages.github.com). Jekyll is a framework used to quickly develop lightweight static webpages to display content. The individual files are all created with Markdown, Liquid Templates, YAML Front Matter, HTML, or CSS. Jekyll is made to make blogging super easy by managing permalinks, categories, pages, and posts. It is also super easy to get up and running with custom themes.

I learned how to use Jekyll through a [Udemy course created by AwesomeInc](https://www.udemy.com/jekyll-and-github-pages/). With that great video series, the [Jekyll docs](https://jekyllrb.com/docs/home/), and a lot of trial and error, developing my own website went fairly smooth and straightforward.

### Getting started with Jekyll

Installing Jekyll was super easy on my Ubuntu 16.04 machine. I just followed the instructions from [this tutorial](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-jekyll-development-site-on-ubuntu-16-04). These are the notes I took from there on installing and creating my first website:

>	1) Installing
>		- sudo apt-get update
>		- sudo apt-get install ruby ruby-dev make gcc
>			- I think I already had make and gcc
>		- sudo gem install jekyll bundler
>			- use Ruby's gem package manager to install Jekyll & the dependency managing bundler
>	2) Open the firewall
>		- sudo ufw status
>			- mine is inactive
>	3) Create a New Development Site
>		- jekyll new testDevSite
>		- bundle install
>			** I didn't do this because it looked like new did it already
>	4) Start Jekyll Web Server
>		- jekyll serve https://localhost:4000
>			- or specify the host address in order to browse from local machine
>				- jekyll serve --host=203.0.113.0
>		- access the site at the URL provided in the terminal
>			- http:___.0.0.1:4000

### Choosing a theme

One of the best parts about Jekyll is how easy it is to get up and running with a beautiful looking theme. There is a huge list of good ones at [Jekyll Themes](http://jekyllthemes.org/). I chose to use [Particle by Nathan Randecker](http://jekyllthemes.org/themes/particle/) because I thought that the content was smoothly laid out and that the header was professional yet tech-y and captivating. Check out the simple template at [Particle Demo Website](https://nrandecker.github.io/particle/).

To use the theme you simply download the folder from the website and start personalizing the files right there in that directory.

Once I got up and running I quickly realized that this theme came with some interesting style-sheets already included:

1) [Skeleton](http://getskeleton.com/):

Skeleton is a powerful and simple 12-column responsive boilerplate for making nicely formatted websites. It is how the site manages displaying the content in individual rows that are separated into columns.

2) [Font Awesome 4.7](fontawesome.io):

Font Awesome is a super easy to use, yet massive, library of icons / fonts that can be formatted with CSS. The Particle Theme used these for all of the contact icon links in the header.

3) [Devicon 2.0](konpa.github.io/devicon):

Devicon is a set of icons representing programming languages, design tools, development software, and more. This is what the Particle Theme used for all of the icons in the Expertise Section. Unfortunately I couldn't find a whole lot of logos for developer tools that I actually used. I would have liked to seen: Unity Game Development, Jekyll, JUCE Audio Framework, Music Production Software (Ableton, ProTools), Video Production Software (Premiere Pro, iMovie)

### Customization

Customizing the theme has really been a never ending task as my website grows more and more. It was easy to get converted over from the basic template into a personalized version, which included changing all of the titles, descriptions, links, and paragraphs.

The first larger customization I did was to change the icon for a featured project from an image into a video. Getting it to fit into the layout was quite a headache. I even tried creating a whole video wrapper as in [this YouTube video](https://www.youtube.com/watch?v=N0kKs1fef-c&list=PLHxMDdOvyrzjitCCFqhppldxacWL50jlA&index=4) but it still actually formats poorly sometimes on a skinny display.

Then I went ahead and created another paragraph like "My Expertise", except this time as a more personal "About Me" section. I wanted to give the website a little more personality, so I used skeleton to split that row into two separate columns <code><div class="six columns"></code> and embedded my latest instagram post.

### Creating Posts

With Jekyll and their great [Docs](https://jekyllrb.com/docs/posts/), creating posts was super easy!

#### 1) Create a new folder called <code>/_posts</code>

#### 2) Create a new layout called <code>/_layouts/post.html</code>
	{% raw %}
	---
	layout: default
	---

	<div class="container">
		<h1>{{ page.title }}</h1>
		<time datetime="{{ page.date | date: "%-d %b %y" }}">{{ page.date | date: "%-d %b %y" }}</time>
		<t> - - - {{ page.author }}</t>
		{{ content }}
	</div>
	{% endraw %}

#### 3) Create the first post:

You'll notice that I currently have a lot of Front Matter declaring different variables for the post. It also uses markdown with <code>##</code> to declare a <code><h1></code> and then uses liquid template tags within the markdown to display an <code>img</code> with all the <code>{% raw %}![ {{  }} ]( {{  }}{{  }} ){% endraw %}</code>

	{% raw %}
	---
	layout: post
	title: My First Post!
	author: Brendan Thompson
	date:   2017-08-11 16:30:02 -0400
	permalink: /First-Post
	categories: Testing
	excerpt: My first ever post on my new website
	published: true # jekyll serve --unpublished to view
	image: /assets/img/mountains.jpg
	imageAlt: Mountains
	---

	## Welcome to my new website

	![{{ page.imageAlt }}]({{ site.url }}{{ page.image }})

	This is my first post. I am excited to get this section of the website up and running. Possibly I will re-write all of my JUCE development DevLog out as a section of posts. Maybe another section of posts for JUCE tips & tricks. We'll see what happens
	{% endraw %}

#### 4) Link to the post

This part was harder for me than it should have been because I am streaming this from a github pages project instead of just the main brenthompson2.github.io master branch. I'm sure there is a good way to work around that using liquid template tags and permalinks and stuff, but I don't quite have a grasp on it just yet.

In the end I created a <code>_includes/recent-posts.html</code> section included on the homepage that displays every one of my posts:

	{% raw %}
	<div class="container">

		<div class="home-details">
			<h3> Recent Posts </h3>
		</div>

		<ul class="post-display">
			{% for post in site.posts %}
			<li class="row">
				<div class="six columns images">
	 				<img src="{{ site.url }}{{ post.image }}" alt="{{ post.imageAlt }}">
				</div>

				<div class="six columns">
	 				<h5><a href="{{ site.url }}{{ post.url }}">{{ post.title }}</a></h5>

	 				<time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date_to_string }}</time>
	 				<t> - - - {{ post.author }}</t>
	 				<p>{{ post.excerpt }}</p>
				</div>
			</li>
			{% endfor %}
		</ul>
	</div>
	{% endraw %}



### Conclusion

Creating static webpages with Jekyll is incredibly easy to do, and I highly recommend giving it a try. My website still has a lot more work to be done on it, and with the power of Jekyll I can't wait to see what else it has in store! At this point my number one priorities are creating a navigation header bar, making a projects page, and creating more posts. It would also be cool if I could get the pictures and texts to got back to switching sides like in the demo project. I don't know what I changed but now it is always text first and then the picture

Checkout the code currently streaming this website at [the GitHub repository](https://github.com/brenthompson2/My-Website).











