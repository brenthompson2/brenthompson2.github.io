---
layout: post
title: An Introduction to the JUCE Framework
author: Brendan Thompson
date:   2017-08-13 2:30:00 -0400
permalink: /juce-intro
categories: blog
tags: Audio
excerpt: A simple introduction to the JUCE C++ Audio Development Framework
image: /assets/img/post-images/juce-logo.png
imageAlt: JUCE Logo
image2: /assets/img/post-images/juce.png
image2Atl: JUCE Logo
image-slider: /assets/img/post-images/slider-images/juce-logo-slider.png
---

## What Is JUCE?

[JUCE](https://www.juce.com) is a partially open-source cross-platform C++ application framework, used for the development of desktop and mobile applications. JUCE is used in particular for its GUI and plug-ins libraries

I found JUCE while searching for a good method of creating VST plug-ins for Music Production. In particular, I was looking for a simple way to create a Synthesizer plug-in that I could use when producing music. However, I didn't want to spend the summer bogged down with Signal Processing and Wave Physics. That is where JUCE came in.

## The Projucer

The main JUCE application is called the Projucer. It asks the user to chose between a few different project types, including:
- GUI Application
- Console Application
- Audio Application
- Audio Plug-in

It then creates a large new template project. With all of their built in classes it is a breeze to start inputting and outputting both Audio and MIDI data, processing audio, and creating some really interesting applications.

## Online Resources

There are tons of great tutorials out there that are great for getting started with the Projucer and starting to create small applications. On YouTube there are a bunch of good video tutorials as well. However, I have found that taking steps past the basics can quickly become quite challenging.

## My Current Standing

**[Edit 10/19/17]: See the [JUCE Summer Summary](/juce-summer-project) project page for the updated current standing**

As of this point I am struggling to get the Audio Processing classes to communicate together. Basically, there are a series of objects that are required to start creating advanced applications and smoothly communicating data between the UI and the Audio thread:
- AudioDeviceManager: This is the main object that takes in audio & midi data, sends it to its children, and the outputs it again. In an Audio Application, one of these is a member of the MainContentComponent as it inherits from the AudioAppComponent.
- AudioProcessorPlayer: A much less important class, this is an AudioIODeviceCallback, which must be passed as a pointer to the ADM
- AudioProcessorGraph: This is really the heart of the application. The graph takes in the original signal, routes it throughout the software to all of the different processors, and finishes with an edited audio buffer.
- AudioProcessor: This is where all of the actual audio processing takes place. In a VST, the main project is just one AudioProcessor (probably more of a graph). One processor may be an oscillator while another may be a lowpass filter. Processors link to the UI class, AudioProcessorEditor, and communicate with the interface through Listeners.
- AudioProcessorEditor: The Interface component for an audio processor