---
layout: post
title: JUCE Summer Summary
author: Brendan Thompson
date:   2017-10-19 7:30:00 -0400
permalink: /juce-summer-project
categories: projects
tags: Audio
excerpt: The summary of work from my experience developing JUCE Audio synthesizers during the summer of 2017.
github: https://github.com/brenthompson2/JUCE-Summer-Project

image: /assets/img/project-images/juce-harmonic-synth.png
imageAlt: JUCE Synth
image-slider: /assets/img/project-images/slider-images/juce-harmonic-synth-slider.png
---

### Overview

For the summer of 2017 I wanted to look into developing a software synthesizer, possibly one that could be loaded as a VST plugin into music production software such as a Digital Audio Workstation (DAW). I decided to use JUICE for a few reasons, mainly becuase it focuses on code as opposed to virtual hardware and it appeared to have a pretty legitimate and large user base. However, attempting to get further into the nuts and bolts passed the tutorials quickly became quite challenging. In the end I gained a much better understanding of C++, gained a huge appreciation for how elaborate Audio software is, realized how complicated Synthesizers are, and created some neat applications. The last one I was able to do before school starting back up was an Additive Synthesizer with automatically Harmonized, yet toggle-able, harmonics.

Check out the Development Logs and take a look as I tought myself the advanced topics necessary,
laugh as I struggle through the nitty-gritty of troubleshooting EVERYTHING,
and follow along as the software development process unraveled.

As quickly as it came the summer ended, leaving me with a million more questions than answers. However, it also left me super hungry. I had an absolute blast teaching myself about audio programming, and can most definitely see myself loving to do it for the rest of my life. Between School, Work, my Internship at Awesome Inc, Lacrosse, my German Shepherd Puppy, and everything else going on in my life I am currently unable to get back into it, but I am really looking forward to the day I finally can. As I purchased my textbooks for the semester I also went ahead and bought "Designing Software Synthesizer Plug-Ins in C++: For RackAFX, VST3, and Audio Units" by Will Pirkle, which should be a really good guide for when the time comes for me to get back into it.

<hr>

### Projects

#### 1) Chord Machine:

- Barely got started
- Planned to Automatically play harmonic triads (major, minor, etc)
- List of Oscillator objects added together
- Split into multiple files for Oscillators, Interface, etc
- Toggle On/Off as well as control Volume & Frequency for each Oscillator
- Control Main Volume

#### 2) Harmonic Synthesizer:

- List of Oscillator objects added together
- Split into multiple files for Oscillators, Interface, etc
- Automatically tunes subsequent oscillators to match the fundamental
- Toggle On/Off as well as control Volume & Frequency for each Oscillator
- Control Main Volume

#### 3) Additive Synthesizer:

- List of Oscillator objects added together
- Split into multiple files for Oscillators, Interface, etc
- Toggle On/Off as well as control Volume & Frequency for each Oscillator
- Control Main Volume

#### 4) Additive Manual:

- Hardcoded a List of Oscillators and added them together
- Interface & Processor in same files
- Toggle On/Off as well as control Volume & Frequency for each Oscillator
- Control Main Volume

#### 5) HandlingMidi:

- Began attempting to handle midi signals
- Never tested with an actual MIDI controller

#### 6) AudioManagementTemplate:

- Where most of my time and energy got wasted
- Attemted to use the elaborate juce classes to handle the flow of audio
- See "Current Standing" of this blog post: https://brenthompson2.github.io/JUCE-Intro

#### 7) FirstSynth:

- One oscillator
- Decent layout of the Interface
- Controls for Volume & Frequency of that oscillator

#### 8) MyFirstPlugin:

- Blank Audio Plugin project
- should be able to open it in a DAW but never tested

#### 9) Tutorial6TAP:

- Followed along with a youtube tutorial creating and customizing sliders

<hr>

### Sample Code

#### Oscillator


	void MainContentComponent::getNextAudioBlock (const AudioSourceChannelInfo& bufferToFill)
	{
	    const int numChannels = bufferToFill.buffer->getNumChannels();
	    const int numSamples = bufferToFill.numSamples;
	    double originalAngle = currentAngle;
	    // For Each Channel
	    for (int channel = 0; channel < numChannels; channel++){
	        float* const buffer = bufferToFill.buffer -> getWritePointer (channel, bufferToFill.startSample);
	        currentAngle = originalAngle;
	        updateAngleDelta();
	        // For Each Sample
	        for (int sample = 0; sample < numSamples; sample++){
	            // nextSample = (randomGen.nextFloat() * 2.0f - 1.0f); // For Randomly generated White Noise
	            const float nextSample = (float) std::sin (currentAngle);
	            currentAngle += angleDelta;
	            buffer[sample] = nextSample * volumeLevel;
	            if (!(sample % 100)) { std::cout << buffer[sample] << std::endl;}
	        }
	    }
	}

This is the oscillator code from the `FirstSynth` project. Basically, the main object in JUCE calls this `getNextAudioBlock()` function constantly and passes it an audio buffer. For this oscillator implementation, the code goes through every channel and every sample and fills the buffer with `nextSample`. As can be seen in the commented out line, filling the buffer with random values generates white noise. In order to generate a Sine Wave, the buffer needs to be filled with values related to the change in the angle for each sample given the `currentFrequency`. This is managed in the `updateAngleDelta()` function below. As the projects got more advanced, the oscillator functions had to get exponentially more elaborate. For the synthesizers with multiple voices, the projects used an array of these components where each has its own member variables such as `angleDelta`, `currentFrequency`, `volumeLevel`, and many others.

	void MainContentComponent::updateAngleDelta(){
        // number of cycles necessary per each output sample, multiplied by length of sine wave cycle (2pi radians)
        const double cyclesPerSample = currentFrequency / currentSampleRate;
        angleDelta = cyclesPerSample * 2.0 * double_Pi;
    }

#### Drawing the UI


    void MainContentComponent::resized()
    {
        Rectangle<int> area(getLocalBounds());
        // Header & Footer Areas
        const int headerFooterHeight = 36;
        header.setBounds (area.removeFromTop (headerFooterHeight));
        footer.setBounds (area.removeFromBottom (headerFooterHeight));
        // Sidebar Area
        const int sidebarWidth = 80;
        sidebar.setBounds (area.removeFromLeft (sidebarWidth));
        // Content Area
        const int contentItemHeight = 80;
        Rectangle<int> contentOne(area.removeFromTop (contentItemHeight));
        Rectangle<int> contentTwo(area.removeFromTop (contentItemHeight));
        mainVolumeSlider.setBounds(contentOne.getX() + sidebarWidth, contentOne.getY(), contentOne.getWidth() - sidebarWidth, contentOne.getHeight()); // (getWidth() / 2) - 60, getHeight() / 2, 80, 100);
        frequencySlider.setBounds(contentTwo.getX() + sidebarWidth, contentTwo.getY(), contentTwo.getWidth() - sidebarWidth, contentTwo.getHeight()); // (getWidth() / 2) + 60, getHeight() / 2, 80, 100);
    }

The JUCE framework has a fun method for drawing the interface. As can be seen in the snippet above from `FirthSynth`, basically each area is a rectangle subtracted from the entire screen area. Then each element simply sets its border boundaries relative to the areas that it is nested within. This `resized()` function is called automatically whenever the window is resized, which includes upon startup. The code bellow shows the dynamic redrawing of the content area for the `Harmonic Synth`. Each element in the `synthArray` gets drawn and automatically sets its bounds relative to the rectangles it creates relative to the other rectangles already within the elements.

	// Paint the Main Content Area
        const int contentItemHeight = 50;
        const int toggleBtnWidth = 40;
        for (int i = 0; i < numActiveSynths; i++){
            Rectangle<int> tempSynthArea(area.removeFromTop (contentItemHeight));
            Rectangle<int> tempToggleArea(tempSynthArea.removeFromLeft (toggleBtnWidth));
            Rectangle<int> emptyContentArea(area.removeFromTop (contentItemHeight));
            // Set the Toggle Button Area
            (synthArray[i].toggleBtnArea).setPosition(tempToggleArea.getX(), tempToggleArea.getY());
            (synthArray[i].toggleBtnArea).setSize(tempToggleArea.getWidth(), tempToggleArea.getHeight());
            (synthArray[i].btnIsActive).setBounds((synthArray[i].toggleBtnArea).getX(), (synthArray[i].toggleBtnArea).getY(), (synthArray[i].toggleBtnArea).getWidth(), (synthArray[i].toggleBtnArea).getHeight());
            // Set The Volume Area
            (synthArray[i].volumeArea).setPosition(tempSynthArea.getX() + toggleBtnWidth, tempSynthArea.getY());
            (synthArray[i].volumeArea).setSize(tempSynthArea.getWidth() - toggleBtnWidth, tempSynthArea.getHeight());
            (synthArray[i].volumeSlider).setBounds((synthArray[i].volumeArea).getX() + sidebarWidth, (synthArray[i].volumeArea).getY(), (synthArray[i].volumeArea).getWidth() - sidebarWidth, (synthArray[i].volumeArea).getHeight());
            // Set The Frequency Area
            (synthArray[i].frequencyArea).setPosition(tempSynthArea.getX() + toggleBtnWidth, tempSynthArea.getY() + (contentItemHeight / 2));
            (synthArray[i].frequencyArea).setSize(tempSynthArea.getWidth() - toggleBtnWidth, tempSynthArea.getHeight());
            (synthArray[i].frequencySlider).setBounds((synthArray[i].frequencyArea).getX() + sidebarWidth, (synthArray[i].frequencyArea).getY(), (synthArray[i].frequencyArea).getWidth() - sidebarWidth, (synthArray[i].frequencyArea).getHeight());
        }

#### Listening to Sliders

    void MainContentComponent::sliderValueChanged (Slider* slider){
        if (slider == &mainVolumeSlider){
           volumeLevel = mainVolumeSlider.getValue();
        }
        if (slider == &frequencySlider){
            currentFrequency = frequencySlider.getValue();
            if (currentSampleRate > 0.0){
                updateAngleDelta();
            }
        }
    }

Again this code is from the `FirstSynth` project where there was only one oscillator. The code for handling changes in the UI sliders is actually quite simple thanks to the JUCE framework.

### Development Logs

I created detailed development logs during the whole process of learning. Check out [the Development Logs](https://github.com/brenthompson2/JUCE-Summer-Project/tree/master/The%20Development%20Logs) and take a look as I tought myself the advanced topics necessary, laugh as I struggle through the nitty-gritty of troubleshooting EVERYTHING, and follow along as the software development process unraveled.

	- It serves as a guide in case I ever need to redo any of the processes
	- It is a notebook to review and reference while producing audio software
	- There are summaries of research such as articles, tutorials, and videos that I can always reference
	- Hopefully others may use this to guide their own research into Audio Plugin creation
	- As a demonstration of my current ability and potential to grow
