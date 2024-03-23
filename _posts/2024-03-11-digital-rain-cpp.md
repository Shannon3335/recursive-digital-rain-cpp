---
layout: post
title: C++ Matrix Digital Rain
tags: cpp coding project
categories: demo
---
##### Shannon Fernandes

<img src="https://github.com/Shannon3335/recursive-digital-rain-cpp/raw/main/docs/assets/matrixDigitalRain.gif" alt="Digital Rain GIF" style="width:100%; height:200px;" >



## Introduction

We have all seen the digital rain from the Matrix series—the iconic green falling Japanese and Roman characters, and Arabic numerals. Simon Whiteley created the Matrix code. In an interview with CNet, Simon said that the characters were scanned from his wife's Japanese cookbooks further saying "I like to tell everybody that the Matrix code is made of Japanese sushi recipes".It has since become the characteristic mark of the franchise, often used in other pop culture media to portray a hacker looking at 3 screens and saying "I'm in". 

Though I will not be hacking in this project, I will showcase my take on digital rain in C++, implementing modern practices and delving deeper into the standard C++ library.

Here is a table of features I have used from the C++ standard library

| Feature| Purpose |
| -----------| ----------- |
| Vectors| Hold status of async tasks|
| Deques | Implement raindrop of characters|
| Async Function | Print multiple raindrops concurrently|
| Recursion | Raindrop printing logic|
| Mutexes   | Sharing access to cursor pointer|
| Random Device   | Seed random engine|
| Random Engine   | Generate pseudo-random characters|
| Distribution   | Constrain random engine to required range|

## Design and Test

### Coding Practices
#### 1. Github 

Github was essential to aiding my confidence to experiment, being able to roll back breaking changes to the last working version of my code.

<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/githubLogo.png" width=200>
<br>
<em><small>Fig.1.1 Github Logo</small></em>
</div>

#### 2. Using Test Functions to create procedural tests

It was useful to test independent complex pieces of code where possible, to catch issues early, and to prevent a situation where errors or bugs cascade over each other, making debugging a nightmare.

I conducted sanity tests on methods that had to generate random sizes and characters before implementing them in my digital rain logic.

<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/test-functions.png" width=600>
<br>
<em><small>Fig.1.2 Test functions created in TestDigitalRain</small></em>
</div>

#### 3. Using global defines for debugging

I used separate global defines to conditionally run my test code, and to increase the sleep time between print statements. This allowed me to observe the droplets more closely when testing my algorithm.

<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/global-defines-testing.png" width=600>
<br>
<em><small>Fig.1.3 Preprocessor macro TESTING conditionally runs testing code</small></em>
</div>

## Algorithm

There are many ways I visualise the Matrix Digital Rain. I first watched plenty of digital rains created by other people online using coding languages or graphics tools. I slowed these videos down to see whether I could find any patterns in how the characters, rows, or columns moved and changed. 

My best friend when creating this algorithm was the pen and paper. Drawing multiple states of a single raindrop helped me visualise how my algorithm would work, making patterns jump out to me. Noting the X and Y coordinates of a single raindrop's lifetime was how I picked out repeating logic that could be looped, and the relation between the size of the raindrop and its start and end points.

<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/global-defines-testing.png" >
<br>
<em><small>Fig.2.1 My notebook experimenting with different logic</small></em>
</div>


### Digital Droplet Logic

Every droplet begins as a deque of a `random number` of `random characters` representing a column. It was easier for me to visualise an existing raindrop, with a method to print it as opposed to printing the state of a raindrop, conditionally adding a character, and printing it again.

Each deque would be an instance of a `DigitalDroplet` and form a column. A `deque`(short form for double-ended queue) is an indexed sequence container that allows insertion and deletion at both its beginning and end. This perfectly suited my visualisation of every raindrop having a character enqueued from the back, but the top-most character dequeued when the droplet reaches the end of page. While both vectors and deques are dynamically sized data structures, the expansion of a deque is cheaper than that of a vector because it does not copy the existing elements to a new memory location. This comes at the cost of a larger initial memory since a deque holding just one element has to allocate its full internal array (8 times the object size).

I use a `random device` and `random engine` in 2 different functions to generate a random size, and a vector of random characters to fill the raindrop with. Random devices are `non-deterministic`, drawing entropy from hardware or the operating system. The number generated from the random device is used to seed the random engine. Random engines are `pseudo-random`, implementing algorithms, seeded with the value from a random device. Although random devices are preferred where `cryptographic security` is vital, they come with drawbacks, mainly performance, entropy, and implementation. Hence, for most applications other than security, it is recommended to use the more performant random engine, seeded with the random device.

I have opted for the Mersenne Twister Engine, a widely used 32-bit random number generator for this project. This choice is preferred over the obsolete rand() function, which suffers from poor randomness quality and difficulty achieving a uniform distribution within a specified range. I create a uniform distribution to ensure that the numbers generated by the random engine fall within the desired range of ASCII values displayed on the screen.

<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/mersenne-twister-engine.png" width=600>
<br>
<em><small>Fig.2.2 Code to generate random characters</small></em>
</div>

#### 1. Rain Drop

Designing a raindrop to cascade down the screen is my priority for visualising the overall rain. This will form a basis for the later expansion into the rain formation and rain disappearing visualisations.

The above image conveys my thought process, examining the states of the raindrop in terms of a `startpoint` (Y-axis value of the `tail of the droplet`), `endpoint`(Y-axis value of the `head of the droplet`) and `end of page` (where the droplet will `start disappearing`). 

Given the start point, my print function displays the entire raindrop bottom-up at one point of time, starting with the head of the raindrop.

<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/rain-drop-code.png" width=800>
<br>
<em><small>Fig.2.3 Rain drop code snippet</small></em>
</div>

#### 2. Rain Formation

I gave the name Rain Formation to the case where the droplet starts with only the first character on screen.
Since the droplet already exists with a random number of characters, I use a for loop to print the head of the droplet first, going down the Y axis every iteration, revealing another character until all characters are on screen with the tail of the droplet still at (X,0). 

<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/RainFormationLogic.drawio.png" >
<br>
<em><small>Fig.2.4 Rain formation logic</small></em>
</div>
<br>
<br>
<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/new-rain-formation-code.png" width=600>
<br>
<em><small>Fig.2.5 Rain formation code snippet</small></em>
</div>

#### 3. Rain Disappear

*Fig.2.6 INSERT IMAGE OF RAIN DISAPPEARING EXAMPLE*

The logic for the droplet disappearing was comparatively the simplest. When the head of the raindrop reaches the end of the page, dequeue the current head of the droplet using `deque.pop_front()` and print it again with the new head of the raindrop at the end of the page.

<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/rain-disappear.png" width=400>
<br>
<em><small>Fig.2.7 Remove the character at the front of the droplet</small></em>
</div>

#### 4. Recursion

Noticing a repetition in printing the state of a raindrop, I created a recursive function. This function takes in an initial start point, prints the droplet, increments the start point, and calls itself again with the new start point. I could have used a `nested for loop` to repeat this logic, but opted to use recursive logic for its simplicity and clarity, and the easily identifiable end case - the deque of characters being empty. 

<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/rain-logic.drawio.jpg" height=600>
<br>
<em><small>Fig.2.8 Recursion logic flowchart</small></em>
</div>

### Digital Rain Logic

My approach to managing raindrops was shaped by the recursive nature of my print function, which I will delve into more deeply later.

Surrounded by concurrency in GoLang and JavaScript, my solution uses a vector to track the statuses of asynchronous tasks printing raindrops. I have created a proof of concept using both asynchronous tasks and threads but decided to go with asynchronous tasks. Using async tasks in C++ gives an abstracted way to handle concurrency `without managing promises, responses and thread pools`, while giving an `easier way to return values` compared to threads. Threads, however, provide granular control over performance, with the possibility of `true parallelism`, with the tradeoff being higher resource utilisation.

<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/asyncLogic.drawio.png" height=600>
<br>
<em><small>Fig.3.1 DigitalRain controlling mutliple async tasks flowchart</small></em>
</div>
<br>

Each asynchronous task in the vector is associated with a raindrop. When a task is initiated, its corresponding index in the status vector is set to 1. As the recursive function completes its execution, the status value for that index is reset to 0.

<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/async-code.png" height=600>
<br>
<em><small>Fig.3.2 code snippet showing async tasks execution and waiting for response</small></em>
</div>

## Problem-Solving

#### 1. Handling Multiple RainDrops

Once I got the logic of a single raindrop's lifetime, the next step was to replicate this behaviour for multiple raindrops. 

I attempted using a for loop to update the raindrops, but this approach caused a laggy effect as only one raindrop was updated at a time, leaving the others frozen. To address this, I experimented with different methods, keeping concurrency as the last solution, such as adjusting sleep times between print operations and increasing the sleep time between recursive calls. However, I realised that this approach wouldn't work due to the nested nature of recursive functions.  A single recursive function forms a nested priority stack following a `Last-In-First-Out`(LIFO) order, which monopolised priority and prevented it from passing to a different raindrop's nested stack. 

Ultimately, I fixed the problem by using async tasks to handle the printing of each raindrop. I also tackled the issue of the shared console by using a mutex, which helped coordinate the output of each raindrop. I will go in-depth into this solution in the following section.

#### 2. Sharing Access to the Console Cursor

Printing multiple raindrops led to unexpected behavior with the cursor position, resulting in characters being printed at incorrect locations or causing what I call `ghost characters`. 


<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/cursorLost.png" height=500>
<br>
<em><small>Fig.4.1 Image of cursor getting lost, failing to remove trailing characters</small></em>
</div>
<br>

A mutex is a `synchronisation object` that controls access to a `shared resource` in a concurrent or multithreaded program. To synchronise access to the shared resource: the cursor. I create a mutex in the DigitalRain class, and the same mutex is shared between all DigitalDroplets. The mutex is locked right before the console is accessed and is unlocked when the code escapes the scope in which the mutex was locked.

<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/mutexBasicImage.png" width=300>
<br>
<em><small>Fig 4.2 Basic concept of mutex, Marushchack Sofiia[1]</small></em>
</div>


## Modern C++ 

Utilising modern C++ features such as auto typing, lambdas, vectors, and deques was crucial for completing this project. Despite concerns about my algorithm's complexity, using mutexes, threads, and async introduced me to concurrency programming and more advanced features of modern C++.

## References 

[1] Marushchack, Sofiia. “How to Use Golang Mutex for Concurrent Programming.” MarketSplash, 21 Sept. 2023, marketsplash.com/tutorials/go/golang-mutex/. Accessed 20 Mar. 2024.
