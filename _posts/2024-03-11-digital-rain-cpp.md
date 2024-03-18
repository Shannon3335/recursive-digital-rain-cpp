---
layout: post
title: A Project in Modern C++
tags: cpp coding project
categories: demo
---

## C++ Matrix Digital Rain

### Introduction
We have all seen the digital rain from the Matrix series—the iconic green falling Japanese and Roman characters, and Arabic numerals. Simon Whiteley created the Matrix code. In an interview with CNet, Simon said that the characters were scanned from his wife's Japanese cookbooks further saying "I like to tell everybody that the Matrix code is made of Japanese sushi recipes".It has since become the characteristic mark of the franchise, often used in other pop culture media to portray a hacker looking at 3 screens and saying "I'm in". 

Though I will not be hacking anything in this project, I will be creating my take on the digital rain in C++, implementing modern practices and delving deeper into some features provided by the standard C++ library.

Some of the core features C++ I have implemented in my version of Matrix digital rain are:
- Vectors
- Deques
- Async Tasks
- Mutexes

### Algorithm

There were many ways that I visualised the Matrix Digital rain. I first watched plenty of digital rains created by other people online, created either using coding languages or animation tools. I slowed these videos down to see whether I could find any patterns in the way the characters, rows, or columns moved and changed. 

Finally, my best friend when creating this algorithm was the mighty pen and paper. 
I drew multiple states of the generation of a single rain drop using different concepts, looking for any pattern that jumped out to me.


#### Digital Drop Logic

Noting down the X and Y coordinates of the lifetime of a single raindrop was how I picked up repeating logic that could be looped, and how the size of the string of characters related to the start and end points of the raindrop.


#### Rain Drop
Tackling the logic of a single rain dropping after it was completely on the screen was my first plan of action since the logic of other 2 cases require this first.

*insert image of drawing here*

From the above image, you can see how my brain was thinking of this logic in terms of the startPoint (Y-axis value of the tail of the droplet), the endpoint(Y-axis value of the head of the droplet) and end of page (where the droplet will start disappearing).

When creating code for this logic, I knew that I would like the print function for the droplet to only print the status of the rain drop at one point when given the start point of the droplet.
*insert image of code here*

#### Rain Formation
*insert image of drawing here*


*insert image of code here*


#### Rain Disappear
*insert image of drawing here*
*insert image of code here*



*Insert images of drawings *


*talk about the recursive method of printing the droplet*



Each deque would be an instance of a DigitalDroplet and form a column.
A deque(short form for double-ended queue) is an indexed sequence container that allows insertion and deletion at both its beginning and end. This perfectly suited to my visualisation of every Droplet having a character enqueued from the back, but the top-most character dequeued when the droplet reaches its endpoint. While both the vector and deque are dynamically-sized data structures, the expansion of a deque is cheaper than that of a vector because it does not copy the existing elements to a new memory location. This comes at a cost of a larger initial memory since a deque holding just one element has to allocate its full internal array (8 times the object size).

#### Digital Rain Logic
I came up with 2 ways of implementing the digital rain.
1. Creating a matrix using a 2D Vector:
Each column would represent a droplet on the screen. The droplet would fall vertically. Using a random hex code generator that gets converted to binary, with every binary digit corresponding to a column, if the corresponding binary digit was a 1, a random character generator would push a character to the end of that vector. If the binary digit was a 0, the droplet would continuously fall until the droplet disappeared. There were 2 reaons for me to now follow with this concept:
    - In a 2D array, every nested array is a row. Using the vector method push_back() would not work seamlessly with this concept that would require every nested vector to be a column.
    - Though I found the concept of using a Hex-to-Binary generator that would control the lifetime of every raindrop, this felt overcomplicated
    - This concept because I made another concept that was more intuitive and felt more realistically implementable.

2. Creating an async task for every deque
   This implementation diverges from the previous concept, using a vector of async tasks to handle the printing of each raindrop. 
It was more intuitive since I started drawing out a status diagram for the flow of a droplet in 3 main parts:
   - Rain Formation
   - Rain Fall
   - Rain disappear
   It was easier getting the code to work for a single droplet using the logic I prototyped, and later replicating the same logic for every column using async. I was also able to get the code working using threads, but decided to go with async because that is what I started with *NEED A BETTER REASON TO JUSTIFY THIS*.



###Problem-Solving
####Handling Multiple RainDrops
Once I was able to get the logic of a single raindrop working from formation to disappearing, the next step was to figure out how I would replicate this behaviour for multiple columns. 

The first step was to use a for loop, but this led to a laggy effect since only one raindrop would be updated at a time, leaving the other raindrops frozen in place.

Due to my investigation into JavaScript's asynchronous features, I was inclined to look into concurrency in C++ and whether there was another way to approach this issue. It was intuitive to have a single thread responsible for printing a single droplet. 

*insert code using thread*

*insert code using asyncs*

When a Droplet finishes printing 
*insert drawing of status array used to control status of tasks*

####Sharing Access to the Console Cursor
This worked almost perfectly once I implemented it. However, sharing the console cursor between multiple asynchronous tasks led to quirky behaviour with the cursor getting *lost*. 

*insert image of cursor being lost*

Using a mutex solved this issue. A mutex is a synchronisation object that is used to control access to a shared resource in a concurrent or multithreaded program. To synchronise access to the  cursor, a mutex is created as a static variable in the DigitalRain class, ensuring the same mutex is shared between all instances of DigitalRain and by extension, all DigitalDroplets. The mutex is passed by reference to the print function, where the mutex is locked right before the console is accessed. The mutex is automatically unlocked after it code escapes the scope of the mutex being locked.

*insert code of mutex in DigitalRain*


You can add an image that has been uploaded to the repository in a /docs/assets/images folder.

<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/154-23-5-27-18-45-6m.jpg" width="100" height="100">
