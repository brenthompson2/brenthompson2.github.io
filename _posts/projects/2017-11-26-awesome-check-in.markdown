---
layout: post
title: Awesome Check In
author: Brendan Thompson
date:   2017-11-26 7:00:00 -0400
permalink: /awesome-check-in
categories: Ionic
excerpt: The Awesome Check In mobile application is used by Awesome Inc to check in their guests and alert the appropriate team members
image: /assets/img/project-images/awesome-checkin/checkin-screensaver.png
imageAlt: Ionic Logo
---

### The App

Awesome Inc was having quite an issue when guests would arrive to their facility. Upon entering the building guests would walk up to the first person that they saw and ask them what to do. As it is a co-working space, the guest was often disrupting people who were not team members of Awesome Inc to go track down the appropriate team member. Now, with Awesome Check In, guests are directed to an iPad where they select the program that they are there for, supply their name, email, reason for coming, and select the team members that are expecting them. The app then sends a [Slack](slack.com) message to the designated Awesome Inc employee and directs them to sit down on the couch.

### The Ionic Framework

The app itself was created using the open source [Ionic 3](https://ionicframework.com) hybrid mobile app development framework. I wrote a detailed description of the framework in a [recent blog post]({{ site.url }}/blog/ionic-3). Basically, Ionic leverages the power of a whole multitude of web technologies and compiles it into native code that can run on either ios or android phones. The web to native wrapper is the open source technology called Cordova. The code itself utilizes Angular 2, Bootstrap, Node.js,

### Major Functions

##### Forms

![User Info Form]({{ site.url }}/assets/img/project-images/awesome-checkin/checkin-userinfo.png)

There are two main forms that the user fills out while checking in. The first one asks which team members the user is intending to meet with. They are provided with a check-box list of team members and are allowed to select multiple. If they are just walking in and do not have a meeting, they can select the "Nobody: Set up a Meeting" option. At the end, the messages are sent via slack to the appropriate people or else to the general "Check In" channel. There is a validator which ensures that one of the check boxes has been selected.

The second form asks for some basic information from the user. They are giving three text boxes to fill out: name, email, and reason. All of the formControls are mandatory in order to proceed.

Once properly filled out the form data is passed to each consecutive page until the user is presented with a Confirm page. They have the ability to look over the info the provided as well as the selected team members. Upon clicking submit, the information is sent as a slack message to the corresponding people.

##### Slack Messaging

![Confirm Page]({{ site.url }}/assets/img/project-images/awesome-checkin/checkin-confirm.png)

One of the key functions of the app is that it sends messages to the appropriate Awesome Inc team members via Slack. The [Slack API](https://api.slack.com) was super easy to use and allows for incredibly customizable messages. They even created a beautiful [Message Builder](https://api.slack.com/docs/messages/builder) that allows for live testing of different messages with attachments, buttons, and more.

I created an in depth [blog post]({{ site.url }}/blog/integrating-slack) about the process of sending a message from the Awesome Check In app into Slack, but the process is quite simple. It basically revolves around creating a JSON object in the format specified by the API and sending it in an HTTP POST request. The intended receiver as well as all of the text, images, links, and attachments are included in the one JSON object. The only other part is setting up Incoming Webhooks as a Custom Integration within Slack itself.

##### Idle Timer

![Idle Timer]({{ site.url }}/assets/img/project-images/awesome-checkin/checkin-idle.png)

The most recent addition to the Awesome Check In app was the implementation of an Idle Timer. The application recognizes that it has been idling on the same page for 60 seconds. It then gives the user 10 seconds to choose to continue working on the forms before it automatically returns to the eye catching starting page.

A detailed blog post about implementing the timer has not yet been created as this feature is still under development. Getting it all working properly has been quite a process. Basically the timer is an injectable provider called timer where a new instance gets instantiated for every page and properly started/stopped/restarted when navigating between pages. The provider itself was based off the countdown timer by [Dave Shirman](http://www.codingandclimbing.co.uk/blog/ionic-2-simple-countdown-timer). The hardest part has been managing the different instances, and it still currently has a bug where pressing the back button does not stop the last timer. The two different solutions that are currently being worked on are only creating one instance of the timer and passing it between all of the pages or otherwise overwriting the back-button functionality to call a special function that pauses the timer. Both options are currently being tested, but in the meantime the bug does not cause any real issues and for the most part goes unnoticed.