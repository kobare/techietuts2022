---
layout: post
title:  "How to use ActiveJob for background processing in Rails"
author: Denis Kobare
date:   2023-04-22 13:30:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: concepts
technology: Ruby
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---


### Introduction

Background processing is an essential component of modern web applications. 
With the growing demand for high-performance applications, background processing 
is becoming increasingly important to handle long-running or resource-intensive 
tasks, such as sending emails, processing files, or generating reports. In the 
Ruby on Rails framework, ActiveJob is a convenient and straightforward way to 
perform background processing. ActiveJob provides a unified interface to work 
with different queuing backends, such as Delayed Job, Resque, or Sidekiq. 

In this blog, we will explore how to use ActiveJob for background processing in Rails.


<br>
### What is ActiveJob?

ActiveJob is a high-level interface for declaring, enqueuing, and executing 
background jobs in Rails applications. It is part of the Rails framework and 
abstracts away the differences between different queuing backends, allowing 
developers to switch between them with minimal effort.

ActiveJob defines a simple API that developers can use to define jobs, specify 
parameters, and enqueue them for execution. Once a job is enqueued, ActiveJob 
takes care of dispatching it to the selected queuing backend, and then executing 
it asynchronously. ActiveJob also provides mechanisms for handling retries, 
failures, and monitoring the status of jobs.

ActiveJob supports a wide range of queuing backends, including in-process queuing, 
database-backed queuing, and external queuing systems such as RabbitMQ, Amazon SQS, 
or Google Cloud Pub/Sub. This flexibility makes ActiveJob an excellent choice for 
any Rails application that needs to perform background processing.


<br>
### a). Creating an ActiveJob

To create a new ActiveJob, we can use the <span class="badge">rails generate job
</span> command. This will 
generate a new job file in the <span class="badge">app/jobs</span> directory of 
our Rails application. For example, to create a job called <span class="badge">
NotificationJob</span>, we can run:


<br>
{% highlight terminal %}

$ rails generate job Notification

{% endhighlight %}

<br>
This will create a file called <span class="badge">notification_job.rb</span> in 
the <span class="badge">app/jobs</span> directory, with the following contents:


<br>
{% highlight ruby %}

class NotificationJob < ApplicationJob
  queue_as :default

  def perform(*args)
    # Do something later
  end
end


{% endhighlight %}


<br>
This file defines a new job class called <span class="badge">NotificationJob</span>, 
which inherits from <span class="badge">ApplicationJob</span>, the base class for 
all ActiveJobs in the Rails application. The queue_as method specifies the queue 
in which the job will be enqueued. In this case, we use the <span class="badge">
:default queue</span>, which is the default queue for the selected queuing backend.


The <span class="badge">perform</span> method is the entry point for the job. 
This method will be called asynchronously when the job is executed, and it can 
receive any number of arguments. Inside this method, we can implement the logic 
for the job, which can include database operations, HTTP requests, or any other 
kind of processing.



<br>
### b). Enqueuing a job

To enqueue a job, we can create a new instance of the job class and call the 
<span class="badge">perform_later</span> method on it. For example, to enqueue a 
<span class="badge">NotificationJob</span> with the argument "Hello, world!", we 
can write:


<br>
{% highlight ruby %}

NotificationJob.perform_later("Hello, world!")

{% endhighlight %}


<br>
This will add a new job to the queue, which will be executed asynchronously by 
ActiveJob.

By default, ActiveJob uses the Async queuing backend, which performs background 
processing in the same process as the Rails application. This can be useful for 
development or testing, but it is not recommended for production environments, 
where a separate queuing backend should be used.


<br>
### c). Configuring the queuing backend

ActiveJob supports several queuing backends, each with its own advantages and 
disadvantages. The most common queuing backends are Delayed Job, Resque, and 
Sidekiq, which provide more robust and scalable background processing than the 
default Async backend. In this section, we will explore how to configure 
ActiveJob to use these queuing backends.



<br>
### i). Delayed Job

Delayed Job is a popular queuing backend for Rails applications. It provides a 
simple way to perform background processing using a separate process or thread 
pool. To use Delayed Job with ActiveJob, we need to add the <span class="badge">
delayed_job</span> gem to our Gemfile and run <span class="badge">bundle install
</span>. Then, we can configure ActiveJob to use the Delayed Job backend by 
setting the <span class="badge">queue_adapter</span> option in our application 
configuration:


<br>
{% highlight ruby %}

# config/application.rb
config.active_job.queue_adapter = :delayed_job

{% endhighlight %}


With this configuration, ActiveJob will use the Delayed Job backend to enqueue 
and execute jobs.


<br>
### ii). Resque

Resque is another queuing backend for Rails applications, based on Redis. It 
provides a scalable and fault-tolerant way to perform background processing, with 
support for job priorities, job dependencies, and job retries. To use Resque with 
ActiveJob, we need to add the <span class="badge">resque</span> and 
<span class="badge">resque-scheduler</span> gems to our Gemfile and run 
<span class="badge">bundle install</span>. Then, we can configure ActiveJob to 
use the Resque backend by setting the <span class="badge">queue_adapter</span> 
option in our application configuration:


<br>
{% highlight ruby %}

# config/application.rb
config.active_job.queue_adapter = :resque

{% endhighlight %}


With this configuration, ActiveJob will use the Resque backend to enqueue and 
execute jobs.



<br>
### iii). Sidekiq

Sidekiq is a powerful and popular queuing backend for Rails applications, based 
on Redis and multi-threading. It provides a fast and efficient way to perform 
background processing, with support for job retries, job batching, and job 
scheduling. To use Sidekiq with ActiveJob, we need to add the <span class="badge">
sidekiq</span> gem to our Gemfile and run <span class="badge">bundle install</span>. 
Then, we can configure ActiveJob to use the Sidekiq backend by setting the 
<span class="badge">queue_adapter</span> option in our application configuration:


<br>
{% highlight ruby %}

# config/application.rb
config.active_job.queue_adapter = :sidekiq

{% endhighlight %}


With this configuration, ActiveJob will use the Sidekiq backend to enqueue and 
execute jobs.


<br>
### Conclusion

ActiveJob is a powerful and flexible way to perform background processing in 
Rails applications. It provides a unified interface for working with different 
queuing backends, allowing developers to switch between them with minimal effort. 
By using ActiveJob, we can offload long-running or resource-intensive tasks from 
the main request-response cycle, improving the performance and scalability of our 
applications. With the support for retries, failures, and monitoring, ActiveJob 
makes it easy to build robust and fault-tolerant background processing workflows.


<br>
*Thanks for reading, see you in the next one!*
