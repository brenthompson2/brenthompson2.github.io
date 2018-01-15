---
layout: post
title: Introduction to the Ionic 3 Framework
author: Brendan Thompson
date:   2017-10-02 7:00:00 -0400
permalink: /ionic-3
categories: blog
tags: Ionic
excerpt: An in depth introduction to the advanced software and concepts that come together to create the Ionic 3 hybrid mobile app development framework
image: /assets/img/post-images/ionic-logo.png
imageAlt: Ionic Logo
image2: /assets/img/post-images/newton-giants.jpg
image2Alt: Standing on the Shoulders of Giants
image-slider: /assets/img/post-images/slider-images/ionic-logo-slider.png
---

When I first arrived at Awesome Inc I had only the slightest experience with web development technologies. After producing some decent updates on a few of their different websites, I was thrown my own big project to tackle. I was asked to use the Ionic hybrid mobile framework to develop a mobile application. Getting started was quite the process, as I was still somewhat new to web development and struggling to understand the big complex interconnectedness of advanced technology that was Ionic. It wasn't until really breaking down the theory behind what all was going on that I was able to jump into the nuts and bolts of development.

### What is a Framework?

Software and Web Frameworks are basically a layer of abstraction that provide a standard way to build and deploy applications. They are universal, reusable environments that enable developers to skip past the more complex lower level details of providing a working system and instead allows them to devote their time to larger requirements. The goal of a framework is to provide a one-size-fits-all solution to the basic problems, and may include programs, compilers, code libraries, tool sets, and API's that bring together all of the components and enable development.

##### The Ionic 3 Framework

<img src="{{ site.url }}{{ page.image2 }}" alt="{{ page.image2Alt }}">

Ionic is made up of a whole slew of other frameworks and technologies. Not only does it handle the communication between all of these advanced parts, it also adds some features by itself. It is an open-source SDK for building beautiful, native, and progressive hybrid mobile apps. Don't worry, we'll define some of those terms as we go forward.

Most notably, the Ionic Framework provides these tools:
- Command Line Interface for creating, running, and testing projects
- Application UI elements that wrap to each native platform
- An online application to test and manage applications

##### Other Frameworks Involved

1) [Cordova](https://cordova.apache.org) by Apache

	This is the powerful technology responsible for enabling the Hybrid Development. This open-source tool wraps HTML, CSS, and JavaScript and allows the user to write one set of code to target nearly every phone or tablet on the market. Basically, it is responsible for taking the code written in these web languages and turning it into Java/ObjectiveC/Swift which are the languages required by the actual mobile devices. Adobe made a proprietary version of the Cordova code and calls it PhoneGap.

2) [Angular 2](https://angular.io/docs) <i class="devicon-angularjs-plain colored"></i>

	Angular is another open-source framework. It was specifically designed for creating web applications. It handles navigating between pages, binding data in real time to entities across the entire program, managing dependencies, and so much more.
	I have found myself using Angular for the following things:
		- Importing Components like NavController, NavParams, HttpModule, FormBuilder, and more
		- Conditionals and Iteration using *ngIf and *ngFor
		- Template variables like {% raw %}{{ program.name }}{% endraw %}

3) [Node.js](https://nodejs.org) <i class="devicon-nodejs-plain colored"></i>

	This is the lightweight and efficient JavaScript runtime that uses an event-driven, non-blocking I/O model. Its package ecosystem, npm, is the largest manager of open source libraries in the world. It is necessary to install node.js and npm before Ionic.

### What are all these languages?

1) TypeScript <i class="devicon-typescript-plain colored"></i>

	- open-source, developed and maintained by Microsoft
	- strict, syntactical superset of JavaScript that adds optional static typing

2) JavaScript (ES7, ES6, ES5) <i class="devicon-javascript-plain colored"></i>

	- High-level, dynamic, weakly typed, object-based, multi-paradigm, interpreted programming language
		- multi-paradigm = event-driven, functional, imperative (oop & prototype based)
	- Used to make webpages interactive and provide online programs
	- Supported by all modern web browsers by means of their own JavaScript Engines
		- Based on the ECMAScript specification
		- some don't implement fully while many support additional features
	- JSON = lightweight data-interchange format that JavaScript objects are converted into and sent between the server and the browser
		- easy for humans to read and write & for computers to parse & generate
		- rules:
			- data in name/value pairs
			- data separated by commans
			- {} curly brackets hold objects
			- [] square brackets hold arrays

3) HTML = HyperText Markup Language <i class="devicon-html5-plain-wordmark colored"></i>

	- received from a web server or local storage and rendered into multimedia web pages
	- denoted structural semantics

4) CSS = Cascading Style Sheets <i class="devicon-css3-plain-wordmark colored"></i>

	- used for describing the presentation of a document written in a markup language
	- designed to enable the separation of presentation and content
		- improve content accessibility
		- provide more flexibility and control in the specification of presentation characteristics
		- enable multiple HTML pages to share formatting
		- reduce complexity & repetition in the structural content

5) Sass = Syntactically Awesome Style Sheets <i class="devicon-sass-original colored"></i>

	- "most mature, stable, and powerful professional grade CSS extension language in the world"
	- scripting language that is interpreted or compiled into CSS
	- the newer syntax uses block formatting like that of CSS and is a nested metalanguage (valid CSS is valid SCSS)
	- official implementation open-source and coded in ruby, although other implementations exist (PHP, C w/ libSass, Java w/ JSass & Vaadin)
	- Provides: variables, nesting, mixins, and inheritance

### The Methodologies

1) Hybrid Mobile App Development

	****** Ionic and NativeScript doesn't use the WebView but instead
	- leverages the power of common web technologies (HTML, CSS, JavaScript) to build cross-platform, yet progressively native mobile applications using the platform WebView

	- App Types:
		A) Native App = written in platform specific language and communicated directly with the OS/Device
			(android = Java; iOS = ObjectiveC or Swift; windows = C#)
		B) Web App = written in web languages and communicated to the OS/Device through a Web Browser application
		C) Hybrid App = written in web languages and wrapped in technology that embeds the web view using native styles and can access the native hardware

	- WebView:
		- Apple decided not to have mobile apps run in Safari's mobile browser but instead used UIWebView
			- Now they use WKWebView as the modern WebKit API in iOS8 & OS X Yosemite
		- Android originally relied on the WebKit rendering engine
			- after Android 4.4, each consecutive update of Android's OS brought along a new version of Chromium which uses the Blink rendering engine
			- Since 5.0, WebView is a 'system-level.apk' available in the Google Play store and can update itself in the background
		- Crosswalk is a project by intel that enables the user to deploy a web application with its own dedicated runtime
			- not dependent on third party or platform-dependent WebView
			- can be used on Android, iOS, Linux, and Tizen

	- Benifits:
		- one code goes cross-platform
		- allows web-developers to easily develop mobile apps w/o learning new, native code
		- minimized learning curve and quicker development by leveraging web technology as opposed to native application tooling and compiled languages

	- Drawbacks:
		- slow
		- "The biggest mistake we've made as a company is betting on HTML5 over native" - Mark Zuckerberg
		Rebuttal:
			- only a noticeable difference if many moving parts, lots of animation, and heavy GPU processing

2) Progressive Web Applications

	- use responsive layouts and UI components that look similar to those found in native mobile applications

3) Reactive Programming

	- an asynchronous programming paradigm concerned with data streams and dynamically responding to change

