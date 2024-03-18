---
layout: post
title: A Project in Modern C++
tags: cpp coding project
categories: demo
---

## C++ Matrix Digital Rain

###Introduction
We've all seen the digital rain from the Matrix series—the iconic green falling Japanese and Roman characters, and Arabic numerals. It has since become the characteristic mark of the franchise. 

In an interview with CNet, Simon Whiteley created the Matrix code and said that the characters were scanned from his wife's Japanese cookbooks. He also said, "I like to tell everybody that the Matrix code is made of Japanese sushi recipes".

This project will explore how I implemented my understanding of a matrix digital rain using Modern C++ tools and practices. 
Some of the core features I have implemented in my version of Matrix digital rain are :
- Vectors
- Deques
- Async
- Mutexes


###Algorithm
There were many ways that I initially visualised the Matrix Digital rain. I first watched plenty of digital rains created by other people online, created either using coding languages or animation tools. I slowed these videos down to see whether I could find any patterns in the way the characters, rows, or columns moved and changed. 

I came up with 2 ways of implementing the digital rain.
1. Creating a matrix using 2D Vectors, using a random hex code generator that I would convert to binary, with every binary digit corresponding to a column. If the corresponding binary digit was a 1, a random character generator would push a character to the end of that vector. 
This is my first paragraph..

## This is a Heading

This is a paragraph. Add an empty line to start a new paragraph.

Font can be *Italic* or **Bold**.

Code can be highlighted with ``backticks``

Hyperlinks look like this [GitHub Help](https://help.github.com/).

A bullet list:

- vectors
- algorithms
- iterators

You can add an image that has been uploaded to the repository in a /docs/assets/images folder.

<img src="https://raw.githubusercontent.com/shannon3335/recursive-digital-rain-cpp/main/docs/assets/154-23-5-27-18-45-6m.jpg" width="100" height="100">
