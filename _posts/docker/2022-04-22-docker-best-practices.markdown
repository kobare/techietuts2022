---
layout: post
title:  "Docker Best Practices"
author: Denis Kobare
date:   2022-04-19 12:40:00 +0300
img: /assets/img/svg/docker.svg
categories: more-topics
sub_category: cloud-services
type: microservice-architecture
technology: Docker
permalink: "category/:categories/cloud-services/concepts/:year:month/:title"
---


<br>
1 . Do not run SSH inside of a container.


<br>
2 . Use Minimal base images: A Minimal base image has the minimum essential resources required to run an application. The most common minimal base image is Alpine Linux which is around 2MB compared to Ubuntu which is around 60MB. Minimal base images translate to faster deploys and smaller attack surface.


<br>
3 . Verify the authenticity of any software you install in your images. You may use official apt-get softwares. If you are downloading from unofficial third party repositories, always verify they are signed or have valid checksums. For example:

 - Use official base images. 
 - If you are adding a file remotely you should make sure that the URL is using HTTPS.
 - If you are using a third-party repository, you should always include the right GPG keys before using your package manager to update these packages.
 - You can even pin to specific hashes of a file to ensure that these remote files have not been maliciously modified. 
 - Always use a specific tag on your "from" directive e.g 
       
        from node:17.0.1 
        
   not this: 
   
        from node


<br> 
4 . Never run your applications as root. Always use the user directive inside of the Dockerfile to make sure that you drop privileges of your user.

{% highlight Docker %}

...

# create group and user

RUN groupadd -r your_user && useradd -g your_user your_user

# some images like nodejs already have a generic user node, so you may skip creating a user


# set ownership and permissions

RUN chown -R your_user:your_user /app


# switch to user

USER your_user


CMD node index.js

{% endhighlight %}

If your application has to start as root because it runs on a privileged support, you can map arbitrary ports on your host inside of the container so you that it doesn't need to run with the same port inside of the container as it does outside.

In the case you need to run as root not because of the port, ensure you draw privileges preferably using gosu and not sudo. 


<br>
5 . Identify cacheable units: Order Dockerfile commands from the least to the most frequently changing to take advantage of docker caching for faster builds.


<br>
6 . Exclude unnecessary files and directories when building to reduce the image size: You should remove unnecessary items like auto-generated directories(e.g target, build) and README files. Simply add them in the .dockerignore file in the root of the project.


<br>
7 . Use multi-stage builds to remove build dependencies such as package.json, pom.xml: The multi-stage build feature allows the use of multiple temporary images during the build process but only keeps the latest image as the final artifact.

{% highlight Docker %}

# Build stage

From maven AS build

WORKDIR /app

COPY myapp /app

RUN mvn package


# Run stage

FROM tomcat

COPY --from=build /app/target/file.war /usr/local/tomcat/webapps/

...

{% endhighlight %}

Notice the use of directive - -from=build, to get the files generated from the build stage and copy them to the final image. The final application image is only created in the last stage.  

<br>
8 . Scan the image for security vulnerabilities:



{% highlight terminal %}

# Login to docker hub

$ docker login


# scan app

$ docker scan myapp:1.0

{% endhighlight %}

- The scan uses an up-to date database of vulnerabilities to identify them.

- You can also configure Docker Hub to scan the images automatically when pushed to the repository.


<br>
*Thanks for reading, see you in the next one!*
