---
layout: post
title: Integrating Slack In Ionic
author: Brendan Thompson
date:   2017-09-07 2:30:00 -0400
permalink: /integrating-slack
categories: blog
tags: Ionic
excerpt: Follow along as I describe how to send an Http post request in Angular 2 as is implemented in Ionic hybrid mobile app development
github: https://github.com/ainc/Awesome-Check-In/blob/master/src/pages/confirm/confirm.ts

image: /assets/img/post-images/slackapi-angularjs.jpg
imageAlt: Slack and Angular
image2: /assets/img/post-images/slack-logo.png
image2Alt: Slack Logo
image3: /assets/img/post-images/ionic-logo.png
image3Alt: Ionic Logo
image-slider: /assets/img/post-images/slider-images/slackapi-angularjs-slider.jpg
---

For my recent internship at Awesome Inc I have been doing some hybrid mobile app development using the Ionic framework. The goal is to create a mobile app to constantly have running on an iPad that allows guests to the facility to check in. The app then sends a message to the appropriate person via [Slack](https://slack.com) regarding the arrival of the guest. Now that I have the messaging feature officially integrated, I just have a little more work to do until an official demo version is ready for testing.

When I was attempting to send a message to Slack from my Ionic app I rattled my brain for a good 6 or so hours before I finally figured out a working solution. Every other link online gave a completely different way of doing it. In the end I followed official tutorials on http post requests and customized them to the slack channel I was trying to reach. One huge roadblock that definitely stopped me from being able to figure it out was that I was testing it using <code>ionic serve --lab -lc</code> which emulates both iOS and Android in the Web Browser. It wasn't until actually running it on my LGV20 phone that I realized I had working code all along. I just plugged in the device, enabled debugging on the phone, opened Android Studio, and ran in terminal <code>ionic cordova run android</code>.

Without any further delay, lets dive into the theory...

### What is HTTP POST?

HTTP, Hypertext Transfer Protocol, is the universally agreed upon underlying format used by the World Wide Web, designed to enable a request-response type communication between clients and servers. For example, when a user enters a URL into a web browser, they are actually sending an HTTP command to the web server requesting for it to fetch and transmit the web page. HTTP defines request methods to indicate the desired action to be performed, the two most popular being <code>GET</code> and <code>POST</code>. The <code>POST</code> method requests that a server accepts the data enclosed in the message. Two of its most common uses include uploading files to a website and submitting a completed web form.

### Incoming Webhooks

![{{ page.image2Alt }}]({{ site.url }}{{ page.image2 }})

The reason I started with describing HTTP POST is because that is exactly how the mobile application is going to communicate the message to Slack. With the basic Slack account, a company is allowed to add 10 different Apps or Custom Integrations to their version of Slack. We use Apps such as GitHub, Google Drive, and Trello to be increase productivity and boost the workflow. For sending messages from an external source there is a different type of connection called an [Incoming Webhook](https://api.slack.com/incoming-webhooks) and it is pretty well defined in the Slack documentation. As is described on the Slack website, the messages can get pretty elaborate with specific designated channels, usernames, images, attachments, and much more.

To set up an Incoming WebHook: log into the Slack account, navigate to the App Directory, and click the Manage tab in the top right. Create a new Custom Integration, change whatever setting are preferred, and save a copy of the URL that it provides. In order to ensure that it was set up properly, I was able to successfully run the following curl command in an Ubuntu terminal and see the message show up in Slack.

{% raw %}
	curl -C POST -H 'Content-Type: application/json' \
	--data '{"text": "Integrating Slack Test Message"}' \
	https://hooks.slack.com/services/YOUR/CUSTOM/WEBHOOK/URL
{% endraw %}

### Sending the POST request in Ionic

![{{ page.image3Alt }}]({{ site.url }}{{ page.image3 }})

If you are interested see my [Introduction to Ionic 3]({{ site.url }}/blog/Ionic-3) post for an overview of the elaborate combination of frameworks and languages that come together to create this powerful hybrid mobile app development framework. In this tutorial we will be writing code in HTML and TypeScript that will have its data managed by Angular 2. We will be starting a blank application, ensuring that it runs properly, and then editing just a few of the 20,000+ files that make up a template project.

The syntax for a POST request is basically <code>this.httpObject.post(URL, JSONobject)</code>. We just need to create a button that calls the function to send the post request on an instantiated instance of an http object and with a JSON object. JSON is a lightweight data interchange format for JavaScript that is easy for humans to read and write and easy for machines to parse and generate. That is the format with which the message content is passed.

### Part 1: Creating the new project

Assuming that Ionic is already installed and configured on the system, run the following commands

A) Create the testingSlackIntegration project using the blank template

	ionic start testingSlackIntegration blank

B) Add Android and iOS builds to the file

	ionic cordova platform add android
	ionic cordova platform add ios

C) Ensure that the program builds and runs in the browser.

	ionic serve --lab -lc

The flag <code>--lab</code> gives the browser view a very nice UI while <code>-lc</code> tells console.log() to send its messages to the terminal

D) Ensure that the program builds and runs on a device. **It would have saved me hours if I had known that the code that we will soon be getting into only worked for me when running on the phone, and it does not work in the web browser version.**
	For android, running on the phone just involved plugging in the device, [enabling developer options](https://developer.android.com/studio/debug/dev-options.html), and building the app with <code>ionic cordova run android</code>. I also had [Android Studio](https://developer.android.com/studio/index.html) up and running a blank new project, which was a whole process in itself.

For iOS you need a mac, to run the project in Xcode, and possibly a $99/year developer account. That is the only way I have heard of to get the application onto an apple device, but I have not yet tested it.

### Part 2: Creating the User Interface

This is super simple. Just go ahead and open the file <code>.../testingSlackIntegration/src/pages/home/home.html</code> and add the following button. Notice that upon being clicked it calls the <code>sendMessage()</code> function that will be implemented in part 3.

{% raw %}

	<ion-header>
	  <ion-navbar>
	    <ion-title>
	      Testing Slack Integration
	    </ion-title>
	  </ion-navbar>
	</ion-header>

	<ion-content padding>
	  Click This Button to send a message on Slack!
	  <button ion-button block (click)="sendMessage()">Send Message</button>
	</ion-content>
{% endraw %}

### Part 3: Implementing the sendMessage() function

The following code, which is to be implemented in the <code>home.ts</code> file in the same folder, is the meat of the integration process.

A) Import the module necessary for creating an http request

	import { Http } from '@angular/http';

B) Declare the private member variable in the class

	private ourHttp: Http;

C) In the constructor of the class link <code>ourHttp</code> member variable with an instance of an http object.
	{% raw %}
	constructor(private http: Http) {
		this.ourHttp = http;
	}
	{% endraw %}

D) Create the sendMessage() function.
	{% raw %}
	sendSlackMessage() {
		var url = "https://hooks.slack.com/services/YOUR/CUSTOM/WEBHOOK/URL";
		var messageText =
			{
				"text": "Testing Message from Ionic Application"
			}

		return this.ourHttp.post(url, messageText)
			.subscribe();
	}
	{% endraw %}

Notice that we declare the URL we will be sending the request to and the exact JSON payload that we will be providing alongside. The URL will be the custom one provided by Slack through the Incoming Webhook that we set up for the task. The JSON payload can include all sorts of special features.

Also, super importantly, without the .subscribe() function called the http post will not be sent.

### Part 4: Import the HttpModule component into the project

In order for the project to run properly, the Angular system needs HttpModule to be imported into <code>../testingSlackIntegration/src/app/app.module.ts</code>

A) Import the module in the top of the file

	import { HttpModule } from '@angular/http'

B) Add it to the list of imports in @ngModule:
	{% raw %}
	...
	imports: [
	    BrowserModule,
	    HttpModule,
	    IonicModule.forRoot(MyApp),
	],
	{% endraw %}


### Part 5: Test the Integration!

Build and run the app on the phone using <code>ionic cordova run android</code>. For me, this code did not appear to do anything in the web browser with <code>ionic serve --lab -lc</code>. However, when testing on an actual device this code should have the <code>"text"</code> show up as a message on slack almost instantaneously!

### Conclusion

There feels like a million and a half different version of post requests already out there on the internet, but it took me entirely too long to find an implementation that actually worked. Stay tuned as either in an update of this post or in a new one I will be showing a more advanced version of this code that implements specific usernames and icons as well as dynamic channels, text, and more. Also, I will be writing a post about the Ionic framework in general and the many advanced technologies the come together to create it. First, I have only a few days left to turn my template project into a beautiful working demo to present to the Owners, Directors, and Managers of Awesome Inc.