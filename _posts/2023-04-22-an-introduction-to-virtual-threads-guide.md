---
date: 2023-04-22 00:30:00 +0530
layout: post
title: Project loom - Virtual Threads
subtitle: Introduction to virtual threads and project loom.
description: Java Virtual Threads — An introduction
image: /assets/img/virtual-threads.jpg
optimized_image: /assets/img/virtual-threads.jpg
category: java
tags:
  - java
  - threads
author: ankurmalviya1618
my_prop: 'nil'
---
<h1 style="text-align:center;"> 
Java Virtual Threads — An introduction 
</h1> 

JDK 19 debuts Java virtual threads—a groundbreaking feature designed not only to cut memory usage but also to boost application availability and improve code quality. 

This post offers a straightforward introduction to the concept of Java virtual threads, making it easy to grasp for everyone.

_**Thread Life Cycle**_ <br>
A typical thread follows these steps:

    1. Creation: The thread is spawned as part of a thread pool.
    2. Idle: It sits in the pool waiting for an incoming request.
    3. Task Pickup: When a request arrives, the thread picks it up and makes a call to the backend database to handle the request.
    4. Waiting: The thread then waits for the database to return a response.
    5. Processing: Once the response is received, it processes the data and sends a reply back to the customer.
    6. Recycle: Finally, the thread is returned to the pool for future tasks.

This cycle—from waiting for a request to being recycled—repeats until the application is shut down. 
Notice that the thread is only actively working when it initiates the database call and when it processes the response. 
In the other phases (creation, idle waiting, and returning to the pool), the thread isn’t doing any actual processing. 
In most applications, threads spend the majority of their lifecycle in this waiting state.

_**Platform Threads Architecture**_ <br>
In earlier JVM releases, only one type of thread existed—known as the "classic" or "platform" thread. 
Each time such a thread was created, an operating system thread was dedicated exclusively to it, remaining occupied until the platform thread terminated. 
In essence, there was a strict one-to-one relationship between platform threads and OS threads.

Because of this design, the operating system thread stays locked during phases of the thread’s lifecycle—such as during creation, while waiting, and when recycling—even when no active work is being performed. Given that OS threads are limited and valuable resources, this architecture leads to significant inefficiencies.

_**Virtual Threads Architecture**_ <br>
To make better use of underlying OS threads, JDK 19 introduces virtual threads. In this model, a virtual thread is attached to a platform (or carrier) thread only when it’s performing actual work—specifically during steps like initiating and processing a task. For all other parts of its lifecycle, the virtual thread exists merely as an object in the Java heap, just like any other application object. 
This approach makes virtual threads exceptionally lightweight and efficient.

_**How To Create Virtual Threads?**_ <br>
All the APIs that work with current platform threads, will work with virtual threads as is. However, the APIs to create java virtual threads are slightly different. 

Below is a sample program that creates Java virtual thread:

```java

Runnable task = () -> { System.out.println("Hello Virtual Thread!"); };

Thread.startVirtualThread(task);
```
In this sample program, the virtual thread is created using ‘Thread.startVirtualThread()‘ API. 
This is the easiest API to create Java virtual threads. If you notice, in the above program, we are invoking Thread.startVirtualThread() method by passing a Runnable object as an argument, which is created.

You can also create java virtual threads using the following APIs:

    1. Thread.ofVirtual().start(Runnable);
    2. Thread.ofVirtual().unstarted(Runnable);
    3. Executors.newVirtualThreadPerTaskExecutor();
    4. Executors.newThreadPerTaskExecutor(ThreadFactory);

_**Advantages of Java Virtual Threads**_ <br>
Because of it’s elegant architecture, virtual threads provides several advantages:

    1. Improves application availability
    2. Improves application throughput
    3. Reduces OutOfMemoryError: unable to create new native thread
    4. Reduces application memory consumption
    5. Improves code quality
    6. 100% compatible with Platform Threads

_**What Is the Performance Impact of Virtual Threads? **_ <br>
Java virtual threads are much more lightweight than platform threads. However, in a nutshell:

If your application either have lot of threads or large stack size (i.e. -Xss), then switching to virtual threads would reduce your application’s memory consumption.
If your application is creating new threads frequently (instead of leveraging thread pool), switching to virtual threads can improve your application’s response time.

## Conclusion

I hope this post helped to gain better understanding about java virtual threads – a wonderful addition to our phenomenal Java programming language.

> Happy Coding :)






