---
title: "Sandboxing"
date: 2025-02-25T20:40:00
draft: false
toc: true
images:
tags:
    - Programming
    - Learning
    - Testing
    - Sandboxing
---

I was reading a blog on [Problem-solving](https://emberavenge.bearblog.dev/self-confidence-in-problem-solving/), 
which mentioned decompiling your problem into small micro-programs. This reminded me 
of a concept I utilise daily when I work. It's called sandboxing.

When adding a feature to a program, or creating a new program, I often test out if my algorithm will work 
by using a little sandbox code environment I've created on my second monitor.

Instead of potentially ruining the code in front of me with a bad solution, I'll 
implement the same solution on some test data structure that contains the exact data as 
in the real production program. This allows me to immediately be able to debug my solution too, 
as the sandbox will only contain the exact code I'm focused on.

You can do the same thing when you have a bug in your real code; conceptualise what your real code 
does in your head (in relation to your bug), and implement that again in the sandbox so you can 
know it works or doesn't work. Often, when you re-implement your solution in the sandbox, you'll 
find out why the bug is happening when you feed some test data through it.

That's just a short little thing that I think some people will find helpful - all programmers 
probably do this already but you never know who you can help :)
