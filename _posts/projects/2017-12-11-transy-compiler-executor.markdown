---
layout: post
title: Transy Compiler & Executor
author: Brendan Thompson
date:   2017-12-11 7:00:00 -0400
permalink: /transy-compiler-executor
categories: projects
tags: C++
excerpt: The Transy Language Compiler and Executor was created for the Compiler Construction course during the fall of 2017 at Transylvania University
github: https://github.com/brenthompson2/Compiler-Construction

image: /assets/img/transy-logo-bat.png
imageAlt: Transylvania Raf Logo
image-slider: /assets/img/project-images/slider-images/transy-logo-bat-slider.png
---

### The Transy Language

For more information on the Transy Language, see my complete version of the [Transy Language Specification](/transy-language-spec).

### The Compiler

The compiler itself is a 3-pass compiler. On its first pass it gathers all of the line labels into the LineLabel table. On the second pass it removes all spaces, removes all blank lines, and changes all characters except those in literals to uppercase. It outputs the modified file as `noblanks.txt`. This file is what is read in during the third pass. The compiler parses the command names and calls on the specific command handlers to process the actual command.

#### CompilerDriver.cpp

This is the main driver for the compiler. It takes in the arguments and parses them to get the flags and the filename. Basically, it creates an instance of BRENxCompiler and tells it to prepare for compilation, to compile, and then to shutdown.

If the `-x` flag is set, is calls upon the executor after successfully completing compilation

**Symbolic Constants**

	MAX_NUM_FLAGS 7

#### BRENxCompiler

This is the class defined in Compiler.cpp & Compiler.h.

**Preprocessing**
- While it is instantiating, it passes pointers to the necessary components to each of the command handlers (FileManager, LineManager, MemoryManager, etc.)
- Gets the filename & flags from the CompilerDriver & passes them to the FileManager
- Gives the LiteralManager and the MemoryManager pointers to the FileManager
- Tells the LineManager to sync the labels recorded during the first pass with the lines given read in during the noBlanks pass

**Compiling**
- Gets one line at a time from the FileManager
- Calls on the specified command handler to manage compiling the line
- Keeps track of the number of errors

**When Finished Compiling**
- Tells the MemoryManager to output the `.core` file
- Tells the LiteralManager to output the `.literal` file
- Tells the FileManager & the MemoryManger to set the compilation result
- Calls the destructor for the FileManager in case the `-x` flag was set and it needs to close the files

**Symbolic Constants**

	MAX_NUM_FLAGS 7
	MAX_NUM_LINES_IN_TRANSY_PROGRAM 1000

### Compiler Components

#### FileManager

This is the class defined in FileManager.cpp & FileManager.h.

**Preprocessing**
- Sets the flags that manage which files get deleted after compilation
- Create or open the files (`.transy`, `.obj`, `.noblanks`, `.core`, `.literal`)
- Handles the first pass by parsing for literals and adding them to the LineManager
- Handles the second pass by creating the noblanks file and adding the lines to the LineManager

**Compiling**
- Gets the next line for the Compiler
- Writes the object code for each command out to the `.obj` file

**When Finished Compiling**
- Writes all the values from the MemoryManager out to the `.core` file
- Writes all the literals from the LiteralManager out to the `.literal` file
- Removes the files based off the compilation result and the flags

#### MemoryManager

This is the class defined in SymbolTable.cpp & SymbolTable.h.

**Compiling**
- Stores all IDs in an array of type memorytableObject
- Returns the memoryLocations for each ID when commands need it
- Writes the object code for each command out to the `.obj` file
- Puts constants into memory

**When Finished Compiling**
- Tells the FileManager to output one value at a time to `.core`
- Manages the compilation result (index 1000 of core memory)

**Type Definitions**

memoryTableObject

	- string name
	- unsigned int memoryLocation (which is just the index and therefore unnecessary)
	- double value
	- bool isConstant (necessary when adding a constant instead of a variable)
	- bool isArray (necessary when adding an array)
	- unsigned int size (for arrays)

**Symbolic Constants**

	MAX_NUM_VARIABLES 1001
	NOT_FOUND_IN_ARRAY -1
	UNDEFINED_VALUE 0.123456789
	INDEX_COMPILATION_RESULT 1000
	SUCCESSFULLY_COMPILED 0
	FAILED_COMPILATION 1

#### LineManager

This is the compiler's object defined in lineLabelTable.cpp & lineLabelTable.h.

**Preprocessing**
- Gets each line label from the FileManager
- Maps each transyLineNumber with the objLineNumber from the FileManager
- Syncs the line labels with the closest line after creating the noBlanks

**Compiling**
- Stores all LineLabels in an array of type lineLabelObject
- Maps objLineNumbers to transyLineNumbers for proper compilation error messages
- Returns the obj line number for each label when commands need it

**Type Definitions**

lineLabelObject

	- string labelName
	- int transyLineNumber
	- int objLineNumber (gets mapped when syncing during preprocessing)

**Symbolic Constants**

	MAX_NUM_LINES 1001
	NOT_FOUND_IN_ARRAY -1

#### ExpressionFixConverter & ExpressionConvertingMatrix

This is the compiler's object defined in ExpressionFixConverter.cpp & ExpressionFixConverter.h. It handles converting assignment statements from infix to postfix. It relies heavily on the ExpressionConvertingMatrix to determine what actions to take for each input value, given the current state of the expression.

### The Executor

The executor is significantly simpler than the compiler. It assumes that all of the lines of code are correct, and therefore doesn't handle many errors. It does give a few warnings.

#### ExecutorDriver.cpp

This is the main driver for the executor. It takes in the arguments and parses them to get the flags and the filename. Basically, it creates an instance of BRENxExecutor and tells it to prepare for execution, to execute, and then to shutdown.

**Symbolic Constants**

	MAX_NUM_FLAGS 7

#### BRENxExecutor

This is the main executor class created in Executor.cpp & Executor.h.

**Preparing**
- Sets the flags
- Tells the FileManager to load the files

**Executing**
- Gets one line at a time from the ProgramLineManager & calls the associated command handler
- Keeps track of the number of errors

**Symbolic Constants**

	MAX_NUM_FLAGS 7
	MAX_NUM_LINES_IN_TRANSY_PROGRAM 1000

### Executor Components

#### EFileManager

The main file manager for the executor

Loads all of files

- `.obj` into ProgramLineManager
- `.literal` into LiteralManager
- `.core` into MemoryManager

#### EFileManager

The main file manager for the executor created in EFileManager.cpp & EFileManager.h

Loads all of files

- `.obj` into ProgramLineManager
- `.literal` into LiteralManager
- `.core` into MemoryManager

#### CoreMemory

The main MemoryManager for the executor created in CoreMemory.cpp & CoreMemory.h

- Stores all of the values for the IDs
- Handles array dimensions
- Last few spots are for temporary memory during assignment handling

**Symbolic Constants**

	MAX_NUM_VARIABLES 1001
	MAX_SIZE_TEMP_MEM 20
	INDEX_TEMP_MEM (MAX_NUM_VARIABLES - MAX_SIZE_TEMP_MEM - 1)

#### ProgramLineManager

The main ProgramLineManager for the executor created in ProgramLineTable.cpp & ProgramLineTable.h

- Stores all of the lines of obj code and gives them to the executor
- Stores each line as an array of elements

**Symbolic Constants**

	MAX_NUM_LINES 1000
	MAX_NUM_PARAMETERS 40 (for assignment statements)

#### LoopManager

The main LoopManager for the executor created in LoopManager.cpp & LoopManager.h

- Creates a stack of loops whenever the loop command is found
- Whenever the loop-end command is found, it increments the top runnerValue and tests the conditional
- If the condition succeeds, it goes to the line pointed to by the top loop
- If the condition fails, it pops the top loop off the stack and goes to the next line

**Type Definitions**

loopObject

	int lineNumber
	int runnerAddress
	int startIndexAddress
	int endIndexAddress
	double incrementAmountAddress

**Symbolic Constants**

	MAX_NUM_LOOPS 7


### Shared Components

#### LiteralManager

This is the class defined in literalTable.cpp & literalTable.h. It is used during Compilation and Execution

**Compiling**
- Stores all literals in an array of type literalTableObject
- Returns the memoryLocation for each literal when commands need it

**When Finished Compiling**
- Tells the FileManager to output one literal at a time to `.literal`

**Executing**
- Returns the string for each requested literal memory location

**Type Definitions**

literalTableObject (for compilation)

	- string variableName;
	- string literalString;
	- unsigned int memoryLocation (which is just the index and therefore unnecessary)

**Symbolic Constants**

	MAX_NUM_LITERALS 100
	NOT_FOUND_IN_ARRAY -1
	UNDEFINED_LITERAL 0.123456789

### The Commands

This section is currently the exact same as the [Transy Language Specification](/transy-language-spec)

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

#### nop

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
