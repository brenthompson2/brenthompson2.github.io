---
layout: post
title: Integrating Slack In Ionic
author: Brendan Thompson
date:   2017-09-07 2:30:00 -0400
permalink: /Integrating-Slack
categories: Web-Design
excerpt: Follow along as I describe how to send an Http post request in Angular 2 as is implemented in Ionic Hybrid Mobile App Development
image: /assets/img/slackLogo.png
imageAlt: Slack Logo
image2: /assets/img/ionicLogo.png
image2Alt: Ionic Logo
---

For my recent internship at Awesome Inc I have been doing some hybrid mobile app development using the Ionic framework. The goal is to create a mobile app to constantly have running on an iPad that allows guests to the facility to check in. The app then sends a message to the appropriate person via ![Slack](https://slack.com) regarding the arrival of the guest. Now that I have the messaging feature officially integrated, I just have a little more work to do until an official demo version is ready for testing.

When I was attempting to send a message to Slack from my Ionic app I rattled my brain for a good 6 or so hours before I finally figured out a good solution. Every other link online gave a completely different way of doing it. In the end I followed official tutorials on http post requests and customized them to the slack channel I was trying to reach. One huge roadblock that definitely stopped me from being able to figure it out was that I was testing it using <code>ionic serve --lab -lc</code> which emulates both iOS and Android in the Web Browser. It wasn't until emulating it on my LGV20 phone that I realized I had working code all along. I just plugged in the device, enabled debugging on the phone, and did <code>ionic cordova run android</code>.

Without any further delay, lets dive into the theory...

### What is HTTP POST?

HTTP, Hypertext Transfer Protocol, is the universally agreed upon underlying format used by the World Wide Web, designed to enable a request-response type communication between clients and servers. For example, when a user enters a URL into a web browser, they are actually sending an HTTP command to the web server requesting for it to fetch and transmit the web page. HTTP defines request methods to indicate the desired action to be performed, the two most popular being <code>GET</code> and <code>POST</code>. The <code>POST</code> method requests that a server accepts the data enclosed in the message. Two of its most common uses include uploading files to a website and submitting a completed web form.

### Incoming Webhooks

The reason I start with describing HTTP POST is because that is exactly how the mobile application is going to communicate the message to Slack. With the basic Slack account, a company is allowed to add 10 different Apps or Custom Integrations to their version of Slack. The custom integration necessary for sending messages from an external source is called an ![Incoming Webhook](https://api.slack.com/incoming-webhooks) and is decently well defined in the Slack documentation. As is described on the Slack website, the messages can get pretty elaborate with specific designated channels, usernames, images, attachments, and much more.

To set up an Incoming WebHook, log into the Slack account and find the Manage Apps section. Create the new Custom Integration, change whatever setting are preferred, and save a copy of the URL that it provides. In order to ensure that it was set up properly, I was able to successfully run the following curl command in an Ubuntu terminal and see the message show up in Slack.

{% raw %}
	curl -C POST -H 'Content-Type: application/json' \
	--data '{"text": "Integrating Slack Test Message"}' \
	https://hooks.slack.com/services/YOUR/CUSTOM/WEBHOOK/URL
{% endraw %}

### Sending the POST request in Ionic

![{{ page.image2Alt }}]({{ site.url }}{{ page.image2 }})

See my (not yet written) post about Ionic for an overview of the elaborate combination of frameworks and languages that come together to create the hybrid mobile app development framework. In this tutorial we will be writing code in HTML and TypeScript that will have its data managed by Angular 2. We will be starting a blank application, ensuring that it runs properly, and then editing just a few of the 20,000+ files that make up a template project.

The syntax for a POST request is basically <code>this.httpObject.post(URL, JSONobject)</code>

### Part 1: Creating the new project

Assuming that Ionic is already installed and configured on the system, run the following commands

A) Create the testingSlackIntegration project using the blank template
	<code>ionic start testingSlackIntegration blank</code>

B) Add Android and iOS builds to the file
	<code>ionic cordova platform add android</code>
	<code>ionic cordova platform add ios</code>

C) Ensure that the program builds and runs in the browser. <code>--lab</code> gives it a very nice UI and <code>-lc</code> enables console.log() to display in the terminal
	<code>ionic serve --lab -lc</code>

D) Ensure that the program builds and runs on a device. **It would have saved me hours if I had known that the following method only works when running on the phone, and it does not work in the web browser version.**
	For android, running on the phone just involved plugging in the device, ![enabling developer options](https://developer.android.com/studio/debug/dev-options.html), and building the app with <code>ionic cordova run android</code>. I also had ![Android Studio](https://developer.android.com/studio/index.html) up and running a blank new project, which was a whole process in itself.

	For iOS you need a mac, to run the project in Xcode, and possibly a $99/year developer account. This is the only way I know of to get the application onto an apple device.

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

The following code, which is to be implemented in <code>.../"  "/home.ts</code> is the meat of the integration process.

A) Import the module necessary for creating an http request
	<code>import { Http } from '@angular/http';</code>

B) Declare the private member variable to the class
	<code>private ourHttp: Http;</code>

C) In the constructor of the class link the <code>ourHttp</code> member variable with instance of an http object.
	{% raw %}
	constructor(private http: Http) {
		this.ourHttp = http;
	}
	{% endraw %}

D) Create the sendMessage() function. Notice that we declare the URL we will be sending the request to and the exact JSON payload that we will be providing alongside. Without the .subscribe() function called, the http post will not be sent.
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

### Part 4: Import the HttpModule component into the project

In order for the project to run properly, the Angular system needs HttpModule to be imported into <code>../testingSlackIntegration/src/app/app.module.ts</code>

A) At the top import the module <code>import { HttpModule } from '@angular/http'

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

Build and run the app on the phone using <code>ionic cordova run android</code>. **THIS CODE DID NOT WORK FOR ME WHEN USING <code>ionic serve --lab -lc</code>.** However, when testing on an actual device this code should have the <code>"text"</code> show up as a message on slack almost instantaneously!

### Conclusion

There are a million and a half different version of this code already out there on the internet, but it took me entirely too long to find an implementation that actually worked. However, keep in mind that I am not sure that I was testing using an actual device during that annoying experience. Stay tuned as either in an update of this post or in another I will be showing a more advanced version of this code that implements specific usernames and icons as well as dynamic channels, text, and more.