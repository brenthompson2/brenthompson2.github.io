---
layout: post
title: Flexslider in Jekyll
author: Brendan Thompson
date:   2018-01-18 7:00:00 -0400
permalink: /flexslider
categories: blog
tags: Web-Design
excerpt: Implementing Flexslider in Jekyll in order to display posts and projects
github: https://github.com/brenthompson2/brenthompson2.github.io/blob/master/_includes/displays/posts-flexslider.html

image: /assets/img/post-images/jekyll-logo.png
imageAlt: Jekyll Logo
image-slider: /assets/img/post-images/slider-images/jekyll-logo-slider.png
---

After implementing the [Ideal Image Slider](https://jekylltools.github.io/jekyll-ideal-image-slider-include/examples/) on two different Jekyll site and writing a blog post about it I tested it on mobile and was quite disheartened. Not only did the slider not rotate automatically, the user wasn't even able to navigate it manually. I quickly had to replace it, and what I found was leaps and bounds better. [Flexslider 2](http://flexslider.woothemes.com/) by WooCommerce claims to be "The best responsive slider. Period.", and at least when it comes to jekyll I believe it to be true. Getting set up with Flexslider is very intuitive and it looks beautiful. The customizable captions, nav controls, and thumbnail sliders make it incredibly powerful while it remains simple to implement.

<div class="row">
	<div class="six columns">
		<p><a href="http://davidollerhead.com/blog/2013/08/06/lets-build-a-carousel.html">David Ollerhead</a> gives a pretty good example of creating the slider for pictures, but it didn't work for me the way he set his up. Furthermore, I wanted to use mine to display posts whereas his was just a carousel of pictures. After some tinkering it quickly came together and can be seen to the right displaying all my Recent Posts.</p>

		<h3>Implementing the Slider</h3>

		<p>Setting up the slider was incredibly easy. The steps basically break down into adding and linking to the css & javascript dependencies, creating the sliders, and including them within the page.</p>

		<h2>Step 1: Add the Dependencies</h2>

		<p>There are only 5 different dependency files necessary to get the slider to run and they can all be found in the folder downloaded from the <a href="https://woocommerce.com/flexslider/">official WooCommerce Flexslider page</a>.</p>
	</div>
	<div class="six columns">
		{% include displays/posts-flexslider.html %}
	</div>
</div>

#### The CSS

In the folder downloaded there should be a file called `flexslider.css` which should be added to the Jekyll site in the folder `/assets/css/`

The CSS file:

	flexslider.css

#### The JavaScript

Again in the folder downloaded there should be two `.js` files, both of which should be added to the jekyll site in a new folder called `/assets/js/flexslider`

The two JavaScript files:

	jquery.flexslider.js
	jquery.flexslider-min.js

#### The Navigation Icon Fonts

For the flexslider, the arrows that work as navigation are actually defined as fonts, and therefore must be included in `/assets/css/fonts/`. I was able to get it to work with just two of the four files that got downloaded in the `fonts` folder from Flexslider.

The two Nav Icon Font files:

	flexslider-icon.ttf
	flexslider-icon.woff

## Step 2: Create the Slider

To create the slider it must be initialized in JavaScript and defined in HTML.

#### Initializing the Slider

In a file called `/assets/js/app.js` the slider needs to be initialized, which is where most of the customization comes in. The best way to do this is to find the version of code described on the [flexslider demo site](http://flexslider.woothemes.com/) which will look something like below:

	// Initialise FlexSlider for Carousels
	$(window).load(function() {
	    $('.flexslider').flexslider({
	    animation: "fade",
	    directionNav: true,
	    slideshowSpeed: 5000,
	    animationSpeed: 600,
	    touch: true
	    });
	});

These are the options that I have found:

- `Animation`: "fade" or "slide"
- `controlNav`: "thumbnails" or false (this is for the thumbnail sliders)
- `directionNav`: true or false (this is for the directional arrows)
- `slideshow`: true or false
- `slideshowSpeed`: number in milliseconds
- `animationSpeed`: number in milliseconds
- `animationLoop`: true or false
- `touch`: true or false (for touchscreens)
- `itemWidth`: width in pixels
- `itemHeight`: height in pixels
- `itemMargin`: margin in pixels
- `asNavFor`: used by thumbnail controlNav sliders to select main slider
- `sync`: used by main slider to sync with thumbnail controlNav slider

#### The HTML

In the `/_includes/` folder add the HTML that describes the markup for the slider.

My `flexslider-posts.html`:

	{% raw %}
	<div>
	  	<div class="home-title">
	    	<h3>Recent Posts</h3>
	  	</div>
		<div class="flexslider">
			<ul class="slides">
		  		{% for page in site.categories.blog %}
		  		<li>
		  			<a href="{{ site.url}}{{ page.permalink }}">
			    		<img src="{{ page.image-slider }}">
			    		<p class="flex-caption">{{ page.excerpt }}</p>
			    	</a>
		 		</li>
		  		{% endfor %}
			</ul>
		</div>
		<p style="text-align: center"><a href="{{ site.url }}/posts-page">See All Posts</a></p>
	</div>
	{% endraw %}

The bulk of the include is where the flexslider is dynamically compiled based off the posts. Basically for each post in the site with the category blog a `li` is generated. They are all contained in the `<ul class="slides>"` which is nested in the `<div class="flexslider">`. All of the content is taken from the front matter of the individual posts.

## Step 3: Link to the necessary files

#### The StyleSheet

The main stylesheet needs to be included in the `<head>` of the page. For me, I included the code for linking to it in my `/_includes/head.html` which gets included in my main layout called `page.html`

	<link rel="stylesheet" href="/assets/css/flexslider.css">

#### The Scripts

The scripts must be included at the very end of the `<body>` of the page. For me, I have an include called `scripts.html` where all of my scripts get linked.

{% raw %}
	<!-- Flexslider Scripts
	=================================================-->
		<!-- Google CDN Hosted jQuery  -->
		<script src="//ajax.googleapis.com/ajax/libs/jquery/2.0.2/jquery.min.js"></script>
		<!-- Flexslider Library  -->
		<script src="/assets/js/flexslider/jquery.flexslider-min.js"></script>
		<!-- Initialisation Code  -->
		<script src="/assets/js/app.js"></script>
{% endraw %}

## Step 4: Display the Slider

#### Include the Slider

To implement the slider onto an actual page just include it!

	{% raw %}{% include flexslider-posts.html %}{% endraw %}

## Thats it!

Implementing the Flexslider is a fairly straightforward process, but if you get caught up anywhere feel free to contact me. I customized my captions by overriding the `flex-caption` class in my `/assets/css/main.scss`. Good Luck!