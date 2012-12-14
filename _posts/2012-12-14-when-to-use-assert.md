---
layout: post
title: "When to use assert"
description: ""
category: C
tags: []
---
{% include JB/setup %}
For years I have used assert as a preventive method when programming. It saved me from a lot of bugs. Mostly of them will cost me plenty of time if not intercepted by assert. It is my comfortable tool at hand.

But today we encountered a problem in our program: an error is not handled and the behavior of the program is strange. After looked into the output log and reviewed the code, I find the problem caused by a hardware interface access failure that happens rarely. I did not check the return value of the function but used an assert to catch the failure. So you can see, this only works for debug version. In the release version, the failure will just be omitted and severe problems will occur.

So I need to change the way of my abuse of assert. My new rules are:
    - Only use assert to catch bugs. Examples:
        - An alarm service that supports only EVERYDAY, WEEKDAY, WEEKEND mode. When some of its internal function received a parameter with mode other than above values, I can use assert.
    - Never use assert at circumstances where the result is not 100 percent sure. Examples:
        - memory allocation
        - network access
        - file access
        - hardware interface access
 
