---
layout: post
title: Awesome Check In v2
author: Brendan Thompson
date:   2018-02-18 7:00:00 -0400
permalink: /awesome-check-in-2
categories: projects
tags: Ionic
excerpt: This newly designed version of the Awesome Check In mobile application is used by Awesome Inc to check in their guests and alert the appropriate team members
github: https://github.com/ainc/Awesome-Check-In

image: /assets/img/project-images/awesome-checkin/checkin2-screensaver.png
imageAlt: Ionic Logo
image-slider: /assets/img/project-images/slider-images/checkin2-screensaver-slider.png

sliderData:
- image: "/assets/img/project-images/awesome-checkin/checkin2-screensaver.png"
  caption: "Attention getting screensaver"
- image: "/assets/img/project-images/awesome-checkin/checkin2-home.png"
  caption: "Homepage"
- image: /assets/img/project-images/awesome-checkin/checkin2-program.png
  caption: If the user selected Entrepreneurship they are brought to this page
- image: /assets/img/project-images/awesome-checkin/checkin2-idea.png
  caption: If the user provides their email address this will send them a link to fill out the "I Have An Idea" form
- image: /assets/img/project-images/awesome-checkin/checkin2-team.png
  caption: The user is able to select the team members that they are planning to meet with. These team members are then tagged in the Slack message, sending them a notification.
- image: /assets/img/project-images/awesome-checkin/checkin2-userinfo.png
  caption: The user provides some information about themselves to help keep track of the people checking in
- image: /assets/img/project-images/awesome-checkin/checkin2-confirm.png
  caption: Upon clicking confirm, a message is sent to the Check-In channel on slack and the appropriate team members get notified
- image: /assets/img/project-images/awesome-checkin/checkin2-final.png
  caption: After successfully checking in the user is presented with the Core Values and an option to return home
---

### Checkin V2

The Awesome Check-In Ionic application got an all new interface thanks to the [mockup created by Nathan Faulls]({{ site.url }}/assets/checkin-mockup-nathan-faulls.pdf). Checkout the [original project page]({{ site.url }}/awesome-check-in) for more information about the first version of the Check-In app.

{% include flexslider.html %}

### Global Ionic App Design

Implementing a universal theme for Ionic apps is actual quite simple. Any sass classes defined in `src/app/app.scss` could be used globally throughout the application. This made it super easily to manage the look and feel of the entire application with just a few different project-scoped classes.

`
.cardFont {
	color: $thunder-black;
	font-family: Exo;
	text-align:center;
	text-transform: uppercase;
}
`

`cardFont` was one of the most popular classes used throughout the application. Just about every single page has cards on it that needed to be formatted. This allowed for just one location to control the look of the text due to `cardHeader` and `cardContent` extended the class. With this paradigm, all cards could be styled by simply adding the correct class to the `ion-card-header` and the `ion-card-content`.

{% raw %}
<div class="row">
	<div class="six columns">
		<img src="/assets/img/project-images/awesome-checkin/checkin2-team.png" alt="TeamMemberPage">
	</div>
	<div class="six columns">
		<h5>Card Selection</h5>
		<code>
			.card-red {
				background: $amaranth-red;
			}
		</code>
		<code>
			.card-gray {
				background: $oslo-gray;
			}
		</code>
		<p>To handle the background color of the cards, the theme contains these global classes `card-red` and `card-gray`. With these two background styles declared, it was simple to implement the feature where once a card is clicked it changes colors. This can most obviously be seen in the TeamMembersPage where the cards all start gray but toggle back and forth to indicate selection.</p>
		<code><ion-card (click)="cardClicked(member.tag)" class="card-style" [ngClass]="getSelectStatus(member.tag) ? 'card-red' : 'card-grey'">
		</code>
	</div>
</div>
{% endraw %}

### Fixed Timer

{% raw %}
<div class="row">
	<div class="six columns">
		<h5>Idle Timer</h5>
		<p>Aside from just re-designing the interface, a decent chunk of the underlying code was modified as well. The biggest change to the controller side of the application was with the way that the idle timer works. Before, each page had its own timer that was started upon construction and stopped when navigating to the next page. Unfortunately, when the user clicked the back button the timer wouldn't stop. Therefore the user could have a bunch of timer instances all running at the same time causing errors.</p>
		<p>With the second version of the Check In app the idle timer got seriously cleaned up. Now the home page instantiates the only instance of the timer which then gets passed along to the proceeding pages. Then the timer can be started within the lifecycle event <code>ionViewDidEnter()</code> and stopped within <code>ionViewWillLeave()</code>.</p>
	</div>
	<div class="six columns">
		<img src="/assets/img/project-images/awesome-checkin/checkin2-idle.png" alt="Idle Timer">
	</div>
</div>
{% endraw %}

### Awesome Check In

The app has been checking in guests to Awesome Inc for over a month now. However, it feels like there are always things that can be done to it. The next steps are to get the timer working perfect, get with the designer to make it look sharp, and possibly add the ability for the user to take and send a selfie within the message.

Overall the app has been a lot of fun to build, and I have learned so much about web development. My biggest complaint is that the app runs pretty slow, which is a major drawback of using Ionic and its web-to-native hybrid mobile development technology. I look forward to developing in native code or, better yet, a future where all mobile manufacturers accepted some of the same programming languages.
