---
layout: post
title:  "Dockerize A Rails App"
author: Denis Kobare
date:   2022-04-19 12:40:00 +0300
img: /assets/img/svg/docker.svg
categories: more-topics
sub_category: cloud-services
type: microservice-architecture
technology: Docker/Rails
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---

### Definition

Dockerizing an application, packages it in a docker image that can be run in a container and easily scaled by generating more instances. These containers can also be run in different machines without requiring extra configurations since environment configurations are encapsulated and reporduced.

During the process, we specify everything needed to run the application and the instructions for creating the docker image in a special file called Dockerfile (with no extension).

Since we can only have one application or service per container, the database has to be encapsulated in a separate container. Another file called docker-compose (a yaml file) is used to provide the instructions for spinning up a docker container with the database inter-linked with the app.


NB: Every docker container has it's own networking interface i.e like Operating Systems running in different networks with different IP addresses. If you open the terminal and run the ifconfig command, you will see the docker containers' network interfaces in addition to your local machine's network interface.

<br>
*<u>This document assumes that you already have Docker installed on your operating system.</u>*

<br>
### Create the Dockerfile
At the root of the rails project, create a file called Dockerfile, with no extension. Let's add the following in a stepwise fashion:


<br>
1 . Add an operating system containing the project's version of ruby using the "from" directive. 

Alpine Linux is a lightweight operating system and therefore a great fit for docker containers or any system that is network-based and dedicated for a single purpose only, like in the case of routers.

{% highlight Dockerfile %}

From ruby:3.1.0-alpine

{% endhighlight %}


<br>      
2 . Add database dependencies and other project dependencies.

This example uses a postgres database. Project dependencies may vary depending on the rails project.


{% highlight Dockerfile %}

RUN apk add --update --virtual \
 runtime-deps \
 postgresql-client \
 build-base \
 libxml2-dev \
 libxslt-dev \
 nodejs \
 yarn \
 libffi-dev \
 readline \
 postgresql-dev \
 libc-dev \
 linux-headers \
 readline-dev \
 file \
 imagemagick \
 git \
 tzdata \
 && rm -rf /var/cache/apk/*

{% endhighlight %}
 
Remove the package manager cache: The " && rm -rf /var/cache/apk/* " removes all the installation files that were downloaded for the project dependencies, to free up space for the docker image.
 

<br> 
3 .  Create a directory called "app" to put our project in the root of the docker image.

{% highlight Dockerfile %}

 WORKDIR /app

{% endhighlight %}       
  

<br>       
4 . Copy the contents of the current directory (rails project) to the app directory created in step 3 above. The period stands for the current directory path.

{% highlight Dockerfile %}

 COPY . /app/       

{% endhighlight %} 


<br>
5 . Set gems location.

We want to save our project's gems in a directory called gems at the root of the docker image. This overrides the default location. 

{% highlight Dockerfile %}

 ENV BUNDLE_PATH /gems

{% endhighlight %} 


<br>
6 . Install yarn and then bundle.

{% highlight Dockerfile %}

 RUN yarn install
 RUN bundle install

{% endhighlight %} 

 
<br>      
7 . Define the entry point.

This defines the command to be run when the docker image is started. 

{% highlight Dockerfile %}

 ENTRYPOINT ["bin/rails"]  

{% endhighlight %} 


<br>
8 . Bind the server to 0.0.0.0

{% highlight Dockerfile %}

 CMD ["s", "-b", "0.0.0.0"]

{% endhighlight %}

      
<br>      
9 . Expose the port for the project.

Normally, rails apps run on port 3000

{% highlight Dockerfile %}

 EXPOSE 3000

{% endhighlight %}

   
<br>      
10 . Final Dockerfile.

{% highlight Dockerfile %}

From ruby:3.1.0-alpine
RUN apk add --update --virtual \
 runtime-deps \
 postgresql-client \
 build-base \
 libxml2-dev \
 libxslt-dev \
 nodejs \
 yarn \
 libffi-dev \
 readline \
 postgresql-dev \
 libc-dev \
 linux-headers \
 readline-dev \
 file \
 imagemagick \
 git \
 tzdata \
 && rm -rf /var/cache/apk/*


 WORKDIR /app
 COPY . /app/
 
 ENV BUNDLE_PATH /gems
 RUN yarn install 
 RUN bundle install
 
 ENTRYPOINT ["bin/rails"]
 CMD ["s", "-b", "0.0.0.0"]
 
 EXPOSE 3000  

{% endhighlight %}


<br>
11 . Build the Dockerfile.

cd to the root of your project and run this command. 

{% highlight terminal %}

$ docker build --tag app_name path_to_the_docker_file

{% endhighlight %}

       
e.g 

{% highlight terminal %}

$ docker build --tag myapp .

{% endhighlight %}
       

<br>
If you have an account on docker, you can you can specify your account name like so:

{% highlight terminal %}

$ docker build --tag techietuts/myapp .

{% endhighlight %}       

<br>
You can also specify the version number like so:

{% highlight terminal %}

$ docker build --tag techietuts/myapp:1.0 .

{% endhighlight %}       


<br>
12 . Run the docker app

{% highlight terminal %}

$ docker run -p 3005:3000 myapp

{% endhighlight %}       

*NB: You may need to prepend "sudo" for it work if you have not specified a user in your Dockerfile, but remember that it is bad practice to use privileged permissions as it can allow an attacker to escalate privileges on the host. Always specify a user with least privileges in the Dockerfile.*  
       
The first (local's) port is beeing mapped to the second (container's )port. Open the browser and navigate to the localhost on port 3005.   


You'll get an error because the containerized app has a different networking interface and cannot connect to the database on the local machine. To fix that, we have to spin up other containers that will contain the database servers. Before that, shutdown the server and follow the next step.


<br><br>
### Create Database Servers with docker-compose


1 . Create a docker-compose File

Create a docker-compose.yml file at the root of the project and add the following:


<br>
2 . Define docker-compose file version

It's important to provide the supported version.

{% highlight yaml %}

version: '3.8'    

{% endhighlight %}       


<br>
3 . Define the services

Services can include postgres server, redis server, rails app created in Dockerfile (then referenced using volumes key) etc


{% highlight yaml %}

services:
 db: 
   image: postgres:latest
   environment:
     - POSTGRES_PASSWORD=password
   ports:
     - "5432:5432"
   volumes:
     - "dbdata:/var/lib/postgresql/data"  
              
 redis:
   image: redis:latest
   ports:
     - "6379:6379"

 web:
   build: . 
   ports:
     - "3005:3000"
   depends_on:
     - db
     - redis  
   environment:
     - DATABASE_URL=postgres://postgres:password@db:5432/postgres  
     - REDIS_URL=redis://redis:6379
   volumes:
     - .:/app

volumes:
  dbdata:     

{% endhighlight %}       

NB: Notice how we used volumes key to persist the postgres data so that we dont lose it when the app restarts.

<br>
The final docker-compose.yml file should resemble this:

{% highlight yaml %}

version: '3.8'
services:
 db: 
   image: postgres:latest
   environment:
     - POSTGRES_PASSWORD=password
   ports:
     - "5433:5432"
   volumes:
     - "dbdata:/var/lib/postgresql/data"  
              
 redis:
   image: redis:latest
   ports:
     - "6379:6379"

 web:
   build: . 
   ports:
     - "3005:3000"
   depends_on:
     - db
     - redis  
   environment:
     - DATABASE_URL=postgres://postgres:password@db:5432/postgres  
     - REDIS_URL=redis://redis:6379
   volumes:
     - .:/app

volumes:
  dbdata:     

{% endhighlight %}       


<br>
4 . Finally, run docker-compose

Run this command at the root of the project. This will boot up the app. Open the browser and navigate to localhost on port 3005.


{% highlight terminal %}

$ docker-compose up

{% endhighlight %}       

*NB: You may need to prepend "sudo" for it work if you have not specified a user in your Dockerfile, but remember that it is bad practice to use privileged permissions as it can allow an attacker to escalate privileges on the host. Always specify a user with least privileges in the Dockerfile.* 

You now have the database containers and the app container up and running and mapped together.

<br>
*Thanks for reading, see you in the next one!*
