---
published: false
layout: post
title: Ideal Image Slider in Jekyll
author: Brendan Thompson
date:   2017-11-26 7:00:00 -0400
permalink: /ideal-image-slider
categories: blog
tags: Web-Design
excerpt: Implementing the Ideal Image Slider in Jekyll in order to display posts and projects
image: /assets/img/post-images/jekyll-logo.png
imageAlt: Jekyll Logo
image-slider: /assets/img/post-images/slider-images/jekyll-logo-slider.png
---

<strong>Warning: I no longer recommend the Ideal Image Slider as a method of displaying posts as it is pretty much broken when viewed on mobile. I have written a [Flexslider Post](/flexslider) talking about how to implement the Flexslider instead.</strong>

The [Ideal Image Slider](https://jekylltools.github.io/jekyll-ideal-image-slider-include/examples/) is an open source content slider by [CodeinWP](https://github.com/Codeinwp) that was made for wordpress sites. It was [originally designed](https://github.com/Codeinwp/Ideal-Image-Slider-JS) with the intensions to "create a slider which has just the right amount of features, with no bloat and be easy to extend so that more features can be added as extensions". It was ported to Jekyll by [jekylltools](https://github.com/jekylltools) as an [include](https://github.com/jekylltools/jekyll-ideal-image-slider-include), which is where I learned how to use it. In this post I break down the individual steps of implementing it into a jekyll site. There is also a [ruby plugin version](https://github.com/jekylltools/jekyll-ideal-image-slider) that uses liquid tags but it is not compatible with Github Pages and therefore I have not yet used it.


## **Implementing the Slider**

The process of setting up the slider is not incredibly challenging. The steps basically break down into adding and linking to the css & javascript dependencies, creating the sliders, and including them within the site.

## Step 1: Add the CSS & JS Files

There are 5 different files necessary to get the slider to run. I am using the versions directly behind the demo site, which are hosted in the [assets folder](https://github.com/jekylltools/jekyll-ideal-image-slider-include/tree/gh-pages/assets) of the gh-pages branch.

#### The CSS

Create a new folder `/assets/css/slider/` and add the css, the themes folder, and the default theme

	ideal-image-slider.css
	/themes/default.css

#### The JavaScript

Create a new folder `/assets/js/slider/` and add the js behind the slider itself, the navigation bullets, and the captions

	ideal-image-slider.min,js
	iis-bullet-nav.js
	iis-captions.js

## Step 2: Add the Includes

There are 3 different includes necessary to get the slider to run. Once again I am using the versions directly behind the demo site, which this time are hosted in the [includes folder](https://github.com/jekylltools/jekyll-ideal-image-slider-include/tree/gh-pages/_includes) of the github repository.

#### The HTML

In the `/_includes/` folder add the html behind the slider itself, the scripts, and the styles

	slider.html
	slider_scripts.html
	slider_styles.html

## Step 3: Link to the necessary files

#### The StyleSheet

The main stylesheet needs to be included in the `<head>` of the page. For me, I included the code for linking to it in my `/_includes/head.html` which gets included in my main layout called `page.html`

	{% raw %}{% include slider_styles.html %}{% endraw %}

#### The Includes

The scripts must be included at the very end of the `<body>` of the page. For me, I put the include in my main layout called `page.html` just before the google-analytics include

	{% raw %}{% include slider_scripts.html %}{% endraw %}

## Step 4: Create a Slider

The settings behind each slider is loaded from the folder `/_data/`. Create one if the website doesn't already have it and add the file `sliders.yml`. Hopefully someday there will be a nice way to have the slider dynamically pull images, excerpts, etc from the front matter of the actual posts. However, in the meantime each slider must be statically defined in the yaml file. There are quite a few setting that can be manipulated, and the complete list can be found in the [READEME](https://github.com/Codeinwp/Ideal-Image-Slider-JS/blob/master/README.md) of the original slider.

	- selector: posts_recent
	  captions: true
	  bullets: true
	  images:
	    - src: /assets/img/jekyllLogo.png
	      title: Implementing the Ideal Image Slider in Jekyll in order to display posts and projects
	      href: /ideal-image-slider
	    - src: /assets/img/ionicLogo.png
	      title: An in depth introduction to the advanced software and concepts that come together to create the Ionic 3 hybrid mobile app development framework
	      href: /ionic-3
	    - src: /assets/img/slackLogo.png
	      title: Follow along as I describe how to send an Http post request in Angular 2 as is implemented in Ionic hybrid mobile app development
	      href: /integrating-slack
	    - src: /assets/img/jekyllLogo.png
	      title: Check out this brief description of getting started with Jekyll and getting my first website off the ground
	      href: /website-design
	    - src: /assets/img/JUCE.png
	      title: An Introduction to the JUCE Framework
	      href: /juce-intro
	  settings:
	    height: "400"
	    initialHeight:
	    maxHeight:
	    interval: "'10000'"
	    transitionDuration:
	    effect: "'fade'"
	    disableNav:
	    keyboardNav:
	    previousNavSelector:

## Step 5: Display the Slider

#### Add the Front Matter

In the front matter of pages that need a slider, the slider selectors must be added to the front matter of the page or layout

	---
	image_sliders:
	  - posts_recent
	---

#### Include the Slider

To implement the slider onto an actual page, just include it and specify which slider to display

	{% raw %}{% include slider.html selector="posts_recent" %}{% endraw %}


## Step 6 (Optional): Allow for indexed or archived sliders

In order for the pages to load indexed or archived sliders the following front matter must be included. For example, if there are sliders within post excerpts, this front matter is necessary for them to display properly

	---
	image_sliders_load_all: true
	---

## **Thats it!**

The ideal image slider is a rather simple slider to implement into jekyll and use all over. It has a whole bunch of settings and can easily be modified. If you have any problems implementing it feel free to contact me!