---
layout: post
title: Transy Language Specification
author: Brendan Thompson
date:   2017-12-11 7:00:00 -0400
permalink: /transy-language-spec
categories: blog
tags: C++
excerpt: The Transy Language defined for the Compiler Construction course during the fall of 2017 at Transylvania University
github: https://github.com/brenthompson2/Compiler-Constructin

image: /assets/img/transy-logo-bat.png
imageAlt: Transylvania Raf Logo
image-slider: /assets/img/project-images/slider-images/transy-logo-bat-slider.png
---

### The Transy Language

The language itself was only ever verbally described to the students in class. There were certain specifics that everybody needed to meet, but otherwise the implementations were all individual. It is a loosely typed, procedural language that it Touring Complete. The syntax for the language can most easily be compared to BASIC. It is fairly easy to learn and use, but be careful with those `goto` commands.

The type system:
- ID = variable or constant
- Literal = "string of words" or $literalVariable
- Array = An aggregate data type for IDs

See the project page for my [Transy Compiler & Executor](/transy-compiler-executor) for more information.

### The Commands

#### read

Takes in up to 7 variables to be read in to Core Memory

**example:**

	read firstVar, secondVar, thirdVar

#### write

Takes in up to 7 variables from Core Memory to be written out to the console

**example:**

	write firstVar, secondVar, thirdVar

#### stop

Tells the Executor to finish executing

**example:**

	stop

#### dim

Allocates the space for up to 7 arrays at a time in Core Memory

**example:**

	dim firstArray[10], secondArray[15], thirdArray[20]

#### aread

Reads in values to the array locations in Core Memory between the specified startIndex and endIndex

**example:**

	aread firstArray, 3, 8

#### awrite

Writes the values to the screen from Core Memory in the array locations between the specified startIndex and endIndex

**example:**

	awrite firstArray, 3, 8

#### goto

Starts executing at the line specified by the line label

**example:**

	firstLineLabel:
	goto firstLineLabel

#### loop

Loops while the runnerVariable has not yet crossed the endValue. The runnerVariable must be a variable, while the startIndex, endIndex, and incrementAmount can be variables or constants. It is a pre-test loop

The conditional is based off the incrementAmount.

**Conditions**
- Positive: `<=`
- Negative: `>=`
- Zero: `!=`

**example:**

	loop runnerVariable, startIndex, endIndex, IncrementAmount
		a = a + runnerVariable
	loop-end

#### loop-end

Manipulates the runnderVariable by incrementAmount, based off the last loop encountered by the executor. Then it checks the conditional of that loop. If it succeeds, the program begins executing at the first line in the loop. If it fails, the program continues executing at the next line after loop-end

**example:**

	loop runnerVariable, startIndex, endIndex, IncrementAmount
		a = a + runnerVariable
	loop-end

#### ifa

Jumps to one of the line labels specified based on whether the supplied value is negative, zero, or positive

**example:**

	ifa (varToTest) negativeLabel, zeroLabel, positiveLabel
	negativeLabel:
	lwrite "The value was negative\n"
	goto finishedLabel
	zeroLabel:
	lwrite "The value was zero\n"
	goto finishedLabel
	positiveLabel:
	lwrite "The value was positive\n"
	finishedLabel: stop

#### nop

Does not do anything

**example:**

	nop

#### listo

Prints out the object code for the current program

**example:**

	listo

#### lread

Reads in a $literalVariable to the Literal Table

**example:**

	lread $literalVariable

#### lwrite

Writes the literal to the console. Can take in a $literalVariable or a "literal string". `\n` prints a new line

**example:**

	lwrite "Please supply a literal"
	lread $literalVariable
	lwrite $literalVariable

#### if

Starts executing at the line location specified if the conditional evaluates to true. Otherwise, it continues executing at the next line.

**Operators:**
- `<`
- `<=`
- `=`
- `>`
- `>=`
- `!`

**example:**

	if (firstVar < secondVar) then firstLineLabel
	a = firstVar
	goto finishLabel
	firstLineLabel: a = secondVar
	finishLabel: stop

#### cls

Clears the screen

**example:**

	cls

#### cdump

Prints all of the values in Core Memory between the startIndex and the endIndex

**example:**

	cdump 0, 10

#### subp

Executes the mathematical command represented by the operation and given the ID. It then sets the value to the supplied variable

**Operations:**
- `sin` = sets the variable to the sin of the ID
- `cos` = sets the variable to the cosin of the ID
- `exp` = sets the variable to the base-e exponential function of the ID
- `abs` = sets the variable to absolute value of the ID
- `alg` = sets the variable to the log base 2 of the ID
- `aln` = sets the variable to the natural log of the ID
- `log` = sets the variable to the log base 10 of the ID
- `sqr` = sets the variable to the square root of the ID

**example:**

	subp sin(destinationVariable, sourceValue)

#### Assignment

All basic mathematical expressions can be handled.

**Operators:**
- `+`
- `-`
- `*`
- `/`
- `^`
- `()` = enforced order of operations
- `[]` = direct access into an array index

**example:**

	a[2^5] = 0 + 2 / firstArray[index] * (-10 - b ^ 2)
