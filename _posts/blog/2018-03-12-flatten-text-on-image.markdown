---
published: true
layout: post
title: Flattening Text Onto an Image in Ionic
author: Brendan Thompson
date:   2018-03-12 7:00:00 -0400
permalink: /flatten-text-on-image
categories: blog
tags: Ionic
excerpt: This article explains how to flatten text onto an image in Ionic using a basic HTML Canvas element.
github: http://github.com/brenthompson2/Angel_Central/blob/master/src/pages/text-select/text-select.ts

image: /assets/img/post-images/ionic-logo.png
imageAlt: Ionic Logo
image-slider: /assets/img/post-images/slider-images/ionic-logo-slider.png
---

In order to develop an Ionic application for a client I had to learn how to overlay text onto an image and then flatten it into one new image that the user could share. I spent a ton of time researching this and could not find any real good posts where this was done in Ionic or any other web technology. Luckily a friend of mine recommended that I check out the native HTML Canvas element which is able to handle the task quite nicely.


# Flattening Text Onto an Image

The HTML Canvas element makes this whole process fairly straightforward. Basically the markup needs a `canvas` element to do the creation and an `img` element to display the flattened result. All of the hard work is done in the TypeScript.

## Step 1: Create the Interface

The pattern that I implemented is to have an invisible `canvas` element where the whole thing is drawn and then just a simple `img` element to display the result. There are two important things to note here. First of all, the `src` for the image is an object called `finalImage` which we will declare in the TypeScript later. Secondly, the canvas has the id `#myCanvas` which we will use later to access the element within the code. For testing it may be a wise to remove the style that hides the canvas (`style="display:none;"`).

	<ion-card class="main-display">
    	<img [attr.src]="finalImage">
	</ion-card>
	<canvas #myCanvas style="display: none;"></canvas>


## Step 2: Import Dependencies

In order for the TypeScript to connect to the canvas element we will use the angular core dependencies `ElementRef` & `ViewChild`.

	import { ElementRef, ViewChild } from '@angular/core';

## Step 3: Declare the Member Variables

In order to control the canvas we will need to connect to the element and then manage both the instance of it and the context of it. Notice that we are using `selectedPicture` to store the path to the image we will be using.

	@ViewChild('myCanvas') canvasEl: ElementRef;
    private theCanvas: any;
    private theContext: any;
    private finalImage: any;
    private selectedPicture: any = 'assets/imgs/myImage.jpg'

## Step 4: Prepare the Design Constants

These variables will be used later when drawing the actual canvas. The values could be hardcoded in place, but abstracting these values away into special variables allows for much simpler customization after the fact. These will all make a lot more sense once we start drawing the image and text onto the canvas.


    // Canvas
    private canvasColor = "rgba(255, 255, 255, 1)";
    private pictureWidth = 760;
    private pictureHeight = 1400;

    // Text
    private textX = this.pictureWidth/2;
    private textY = this.pictureHeight - (this.pictureHeight/4);
    private textWrapWidth = this.pictureWidth - (this.pictureWidth/9.00);
    private textWrapHeight = 120;
    private fontStyle = "italic 70px Arial";
    private fontColor = "rgba(255, 255, 255, 1)"; // For fillText
    private borderWidth = 2.2;
    private borderColor = "rgba(0, 0, 0, 1)"; // For strokeText border



## Step 5: Initialize the Canvas

The initialization of the canvas just involves connecting our `theCanvas` element to the `canvasEl` from the interface, setting the dimensions, and connecting to the context. At this point everything is very sequential and relies on the previous steps being completed first. It all starts with the Ionic lifecycle method `ionViewDidLoad()` where we call our new `initializeCanvas()` function.

    initialiseCanvas(){
        this.theCanvas = this.canvasEl.nativeElement;
        this.theCanvas.width = this.pictureWidth;
        this.theCanvas.height = this.pictureHeight;
        if(this.theCanvas.getContext){
            this.theContext = this.theCanvas.getContext('2d');
            this.drawTheImage();
        }
    }

## Step 6: Draw the Image

Step 4 leaves off with connecting `theContext` to the context of the canvas and then calling this new function `drawTheImage()`. Whenever the whole thing needs to be re-drawn, this is the function to call. It starts by clearing the whole thing to start fresh. Then it gives it a background color before drawing the actual image. That math in the parameter of `this.theContext.drawImage(...)` puts the image into the center of the canvas.

    drawTheImage(){
        // Clear Current Image
        this.theContext.clearRect(0, 0, this.theCanvas.width, this.theCanvas.height);

        // Fill background
        this.theContext.fillStyle = this.canvasColor;
        this.theContext.fillRect(0, 0, this.theCanvas.width, this.theCanvas.height);

        // Display the Image
        var img = new Image();
        img.setAttribute('crossOrigin', 'anonymous');
        img.onload = (event) => {
            this.theContext.drawImage(img, this.theCanvas.width/2 - img.width/2, this.theCanvas.height/2 - img.height/2);
            this.drawText();
        };
        img.src=this.selectedPicture;
    }

## Step 7: Draw the Text

Once the image has been drawn it is time to draw the text on top of it. The `fill` is the bulk of the font while the `stroke` is the border. In order to implement text wrapping we have to call another function to draw the lines to fit. I got the text wrapping procedure from an old [canvas tutorial](https://www.html5canvastutorials.com/tutorials/html5-canvas-wrap-text-tutorial/).

    drawText(){
        this.theContext.font = this.fontStyle;
        this.theContext.textAlign = "center";
        this.theContext.fillStyle = this.fontColor; // fill text
        this.theContext.lineWidth = this.borderWidth;
        this.theContext.strokeStyle = this.borderColor; // stroke border
        this.wrapText();
        this.saveCanvas();
    }


	wrapText() {
        var words = this.currentText.text.split(' ');
        var line = '';
        var currentTextY = this.textY;
        for(var n = 0; n < words.length; n++) {
            var testLine = line + words[n] + ' ';
            var metrics = this.theContext.measureText(testLine);
            var testWidth = metrics.width;
            if (testWidth > this.textWrapWidth && n > 0) {
                this.theContext.fillText(line, this.textX, currentTextY); // fill text
                this.theContext.strokeText(line, this.textX, currentTextY); // stroke border
                line = words[n] + ' ';
                currentTextY += this.textWrapHeight;
            }
            else {
                line = testLine;
            }
        }
        this.theContext.fillText(line, this.textX, currentTextY); // fill text
        this.theContext.strokeText(line, this.textX, currentTextY); // stroke border
      }

## Step 8: Save the Canvas

Once the canvas is drawn successfully simply set the `finalImage` member variable to the canvas as a url.

	this.finalImage = this.theCanvas.toDataURL();


# All Done!

Flattening text onto an image is really a rather straightforward process. Basically just connect to the canvas element from the interface template, get the context of it in order to draw the image and the text, and once it is ready just save the canvas out to an image. If you have any questions feel free to contact me. Also, there are a lot of helpful basic Canvas resource online. Good luck!