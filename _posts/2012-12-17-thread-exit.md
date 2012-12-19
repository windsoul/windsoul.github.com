---
layout: post
title: "Thread Exit"
description: ""
category: programming
tags: [thread]
---
{% include JB/setup %}
We have used a thread to simulator our NAND flash based storage device for a long time. Recently a troublesome issue is raised when we planed to add the random power lost feature.  

Usually the device has a reset interface so that host can power down and then bring up it. 

When these two features are mixed, the problem happened. Below are the flow:
    - set a conditional power lost point for device when it is initializing.
	- thread will exit when power lost point reached (thread_exit) [1]
    - reset the device several times to trigger the power lost
        - send message to device for shutdown (semaphore_post)
             - thread will exit when receive this message (thread_exit) [2]
        - wait the device shutdown. (thread_join) [3]
        - reinitializate the device. (thread_create)
    - wait the device shutdown(thread_join). [4] <-- sometimes

It looks like perfect: two exits and two waits. But the problem is that sometimes device has already been shutdown before send message to it. So the [2] will not be called. In this instance, [4] will not return forever.

It took us a lot of time to think about and resolve the problem.

