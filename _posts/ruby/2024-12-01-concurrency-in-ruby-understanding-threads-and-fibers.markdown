---
layout: post
title:  "Concurrency in Ruby: Understanding Threads and Fibers"
author: Denis Kobare
date:   2024-12-01 14:30:00 +0300
img: /assets/img/svg/ruby.svg
categories: programming
sub_category: ruby
type: concepts
technology: Ruby
permalink: "category/:categories/ruby/concepts/:year:month/:title"
---


### Introduction

Ruby is a versatile and dynamic programming language known for its elegant 
syntax and developer-friendly features. One of the key aspects that can 
significantly enhance the performance of Ruby applications is concurrency. In 
this section, we'll delve into concurrency in Ruby, exploring the use of threads 
and fibers, and provide practical insights on when and how to use them 
effectively in various scenarios.



<br>
### What is Concurrency?

Concurrency is the ability of a program to handle multiple tasks simultaneously, 
making the most of available resources and improving overall efficiency. In a 
multi-core world, concurrency is essential to fully utilize the power of modern 
processors.

Ruby, like many other programming languages, provides mechanisms to achieve 
concurrency. Two primary options in Ruby for achieving concurrency are threads 
and fibers.



<br>
### Threads in Ruby

A thread is the smallest unit of execution in a program. Threads in Ruby allow 
you to run multiple tasks concurrently, which can be particularly useful for 
I/O-bound operations, such as handling multiple network requests or file 
operations.


Here's a simple example of using threads in Ruby:

<br>
{% highlight ruby %}

# Creating threads
thread1 = Thread.new do
  5.times { |i| puts "Thread 1: #{i}" }
end

thread2 = Thread.new do
  5.times { |i| puts "Thread 2: #{i}" }
end

# Waiting for threads to finish
thread1.join
thread2.join

puts "All threads have finished."

{% endhighlight %}


<br>
In this example, we create two threads, each of which prints its own count. We 
then use the join method to wait for the threads to finish before printing "All 
threads have finished." This ensures that the main thread waits for the other 
threads to complete before the program exits.



<br>
### When to Use Threads

Threads are ideal for scenarios where your program needs to handle multiple 
tasks simultaneously, especially when those tasks involve I/O operations. For 
instance, if you're building a web server, threads can be used to handle 
incoming requests concurrently, ensuring that the server remains responsive to 
multiple clients.

However, it's essential to be cautious when using threads in Ruby. Ruby uses a 
Global Interpreter Lock (GIL) that prevents multiple threads from executing Ruby 
code in parallel. This means that threads in Ruby are suitable for I/O-bound 
operations but may not be as effective for CPU-bound tasks.



<br>
### Fibers in Ruby

Fibers are a lighter-weight alternative to threads. They allow you to achieve 
concurrency by providing cooperative multitasking, where control is explicitly 
transferred between fibers.

A fiber can be paused and resumed, allowing you to switch between different 
tasks without the overhead of creating new threads. This makes fibers an 
excellent choice for scenarios where you need to manage a large number of 
lightweight tasks.

Here's a simple example demonstrating the use of fibers in Ruby:

<br>
{% highlight ruby %}

# Creating fibers
fiber1 = Fiber.new do
  5.times do |i|
    puts "Fiber 1: #{i}"
    Fiber.yield
  end
end

fiber2 = Fiber.new do
  5.times do |i|
    puts "Fiber 2: #{i}"
    Fiber.yield
  end
end

# Resuming fibers
5.times do
  fiber1.resume
  fiber2.resume
end

puts "All fibers have finished."

{% endhighlight %}


<br>
In this example, we create two fibers, each of which prints its own count. We 
use Fiber.yield to pause a fiber and switch to the other fiber, ensuring that 
both fibers get a chance to execute.



<br>
### When to Use Fibers

Fibers are particularly useful when you need to manage a large number of 
lightweight tasks that can be efficiently scheduled. For example, if you're 
building a web crawler that needs to process a large number of URLs, using 
fibers can help you manage the concurrent processing of these URLs without the 
overhead of creating a thread for each one.

Fibers are also beneficial when you want more fine-grained control over 
concurrency. Since fibers are cooperative, you have explicit control over when 
tasks yield control, which can be advantageous in certain scenarios.



<br>
### Conclusion

Concurrency is a powerful tool that can significantly improve the performance of 
your Ruby applications. Understanding the differences between threads and fibers, 
and knowing when to use each, is essential for writing efficient and responsive 
concurrent programs.

When dealing with I/O-bound tasks, threads can be a good choice due to their 
simplicity and the ability to handle multiple operations concurrently. On the 
other hand, fibers shine when dealing with lightweight tasks that require 
fine-grained control over concurrency and where the overhead of creating threads 
might be a concern.

By mastering concurrency in Ruby, you'll be able to build more scalable, 
responsive, and efficient applications, taking full advantage of the 
capabilities of modern hardware. Remember to consider the specific requirements 
of your application and choose the concurrency mechanism that best suits your 
needs. Happy coding!



<br>
*Thanks for reading, see you in the next one!*
