---
layout: post
title: Awesome Check In
author: Brendan Thompson
date:   2017-11-26 7:00:00 -0400
permalink: /awesome-check-in
categories: projects
tags: Ionic
excerpt: The Awesome Check In mobile application is used by Awesome Inc to check in their guests and alert the appropriate team members
image: /assets/img/project-images/awesome-checkin/checkin-screensaver.png
imageAlt: Ionic Logo
image-slider: /assets/img/project-images/slider-images/checkin-screensaver-slider.png
---

### The App

The first time that I entered Awesome Inc I went straight up to the first person I saw, introduced myself, and said that I was there for a meeting about an internship opportunity. The lady then got up and found the Director of Operations as I nervously waited on the couch just inside the front door. This may sound fine and dandy, but it was actually a huge problem that Awesome Inc had been dealing with. Guests to the facility would approach the person at the closest desk to the door and expect them to be some sort of secretary. However, as it is a co-working space, the people being disrupted more often than not were not even actually team members of Awesome Inc. Now, with Awesome Check In, guests are directed to an iPad where they select the program that they are interested in; supply their name, email, and reason for coming; and then select the team members that are expecting them. The app then sends a [Slack](slack.com) message to the designated Awesome Inc employee and directs them to sit down on the couch. Thanks to Awesome Check In, that person working hard in the Awesome Inc co-working space is no longer being interrupted by every guest to walk in the door.

### The Ionic Framework

The app itself was created using the open source [Ionic 3](https://ionicframework.com) hybrid mobile app development framework. I wrote a detailed description of the framework in a [recent blog post]({{ site.url }}/ionic-3). Basically, Ionic leverages the power of a whole multitude of web technologies and compiles it into native code that can run on both ios and android phones. The web to native wrapper utilizes the open source technology called Cordova while the code itself is managed by Angular 2, Bootstrap, Node.js, and more.

### Major Functions

{% raw %}
<div class="row">
	<div class="six columns">
		<h5>Forms</h5>
		<p>There are two main forms that the user fills out while checking in. The first one asks which team members the user is intending to meet with. They are provided with a check-box list of team members and are allowed to select multiple. If they are just walking in and do not have a meeting, they can select the "Nobody: Set up a Meeting" option. At the end, the messages are sent via slack to the appropriate people or else to the general "Check In" channel. There is a validator which ensures that one of the check boxes has been selected.</p>
		<p>The second form asks for some basic information from the user. They are giving three text boxes to fill out: name, email, and reason. All of the formControls are mandatory in order to proceed.</p>
		<p>Once properly filled out the form data is passed to each consecutive page until the user is presented with a Confirm page. They have the ability to look over the info they provided as well as the selected team members. Upon clicking submit, the information is sent as a slack message to the corresponding people.</p>
	</div>
	<div class="six columns">
		<img src="/assets/img/project-images/awesome-checkin/checkin-userinfo.png" alt="User Info Form">
		<!-- ![User Info Form]({{ site.url }}/assets/img/project-images/awesome-checkin/checkin-userinfo.png) -->
	</div>
</div>
{% endraw %}

{% raw %}
<div class="row">
	<div class="six columns">
		<img src="/assets/img/project-images/awesome-checkin/checkin-confirm.png" alt="Confirm Page">
		<!-- ![Confirm Page]({{ site.url }}/assets/img/project-images/awesome-checkin/checkin-confirm.png) -->
	</div>
	<div class="six columns">
		<h5>Slack Messaging</h5>
		<p>	One of the key functions of the app is that it sends messages to the appropriate Awesome Inc team members via Slack. The <a href="https://api.slack.com" target="blank">Slack API</a> was super easy to use and allows for incredibly customizable messages. They even created a beautiful <a href="https://api.slack.com/docs/messages/builder" target="blank">Message Builder</a> that allows for live testing of different messages with attachments, buttons, and more.</p>
		<p> I created an in depth <a href="/integrating-slack">blog post</a>about the process of sending a message from the Awesome Check In app into Slack. It basically revolves around creating a JSON object in the format specified by the API and sending it in an HTTP POST request. The JSON object includes the intended recipient as well as all of the text, images, links, and attachments for the message. The only other part is setting up Slack itself to accept Incoming Webhooks as a Custom Integration.</p>
	</div>
</div>
{% endraw %}

{% raw %}
<div class="row">
	<div class="six columns">
		<h5>Idle Timer</h5>
		<p>The most recent addition to the Awesome Check In app was the implementation of an Idle Timer that recognizes when it has been idling on the same page for 60 seconds. It then gives the user 10 seconds to choose to continue working on the forms before it automatically returns to the home page.</p>
		<p> A detailed blog post about implementing the timer has not yet been created as this feature is still under development. Getting it all working properly has been quite a process. Basically the timer is an injectable provider called timer where a new instance gets instantiated for every page and started/stopped/restarted when navigating between pages. The provider itself was based off the countdown timer by <a href="http://www.codingandclimbing.co.uk/blog/ionic-2-simple-countdown-timer" target="blank">Dave Shirman</a>. The hardest part has been managing the different instances, and it still currently has a bug that does not cause any real issues and for the most part goes unnoticed. Pressing the back button does not stop the last timer, and I am still testing two options for fixing the issue. One solution is to only create one instance of the timer and pass it between all of the pages while another fix is to manipulate the back-button functionality to also call a function on the page that pauses the timer.</p>
	</div>
	<div class="six columns">
		<img src="/assets/img/project-images/awesome-checkin/checkin-idle.png" alt="Confirm Page">
		<!-- ![Idle Timer]({{ site.url }}/assets/img/project-images/awesome-checkin/checkin-idle.png)-->
	</div>
</div>
{% endraw %}

### Awesome Check In

The app has been checking in guests to Awesome Inc for over a month now. However, it feels like there are always things that can be done to it. The next steps are to get the timer working perfect, get with the designer to make it look sharp, and possibly add the ability for the user to take and send a selfie within the message.

Overall the app has been a lot of fun to build, and I have learned so much about web development. My biggest complaint is that the app runs pretty slow, which is a major drawback of using Ionic and its web-to-native hybrid mobile development technology. I look forward to developing in native code or, better yet, a future where all mobile manufacturers accepted some of the same programming languages.
