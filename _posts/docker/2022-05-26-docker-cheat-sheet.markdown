---
layout: post
title:  "Docker Cheat Sheet"
author: Denis Kobare
date:   2022-05-26 17:05:00 +0300
img: /assets/img/svg/docker.svg
categories: more-topics
sub_category: cloud-services
type: microservice-architecture
technology: Docker
permalink: "category/:categories/cloud-services/concepts/:year:month/:title"
---


<br>
1 . List running containers

{% highlight terminal %}

$ docker ps

{% endhighlight %}


<br>
2 . List all containers

{% highlight terminal %}

$ docker ps -a

{% endhighlight %}


<br>
3 . Start, restart, stop or kill a docker container

{% highlight terminal %}

$ docker start container_id

$ docker restart container_id

$ docker stop container_id

$ docker kill container_id

{% endhighlight %}


<br>
4 . Start a container and keep it running in the background
Detached mode runs containers in the background.

{% highlight terminal %}

$ docker start container_id -d

# Through docker-compose
$ docker-compose up -d

{% endhighlight %}


<br>
5 . Delete container

{% highlight terminal %}

$ docker rm container_id

{% endhighlight %}


<br>
6 . Delete all stopped containers

{% highlight terminal %}

$ docker rm $(docker ps -a -q)

{% endhighlight %}


<br>
7 . Kill all running containers

{% highlight terminal %}

$ docker kill $ (docker ps -q)

{% endhighlight %}


<br>
8 . Show container logs

{% highlight terminal %}

$ docker logs container_id

{% endhighlight %}


<br>
9 . Tail/follow container logs

{% highlight terminal %}

$ docker logs -ft container_id

{% endhighlight %}


<br>
10 . Tail/follow container logs

{% highlight terminal %}

$ docker logs -ft container_id

{% endhighlight %}


<br>
11 . Save a running container as an image

{% highlight terminal %}

$ docker commit -m “commit message” -a “author” container_id username/image_id: tag

{% endhighlight %}


<br>
12 . Run a command in a container

{% highlight terminal %}

$ docker exe -ti container_id command.sh

{% endhighlight %}


<br>
13 . Get shell access to a docker container

{% highlight terminal %}

$ docker exec -it container_id sh

{% endhighlight %}


<br>
14 . Update your current package list

{% highlight terminal %}

$ apk update

{% endhighlight %}


<br>
15 .  Install a package

{% highlight terminal %}

$ apk add package_name

# Example
# Install micro
# A terminal based text editor with syntax highlighting

$ apk add micro

{% endhighlight %}




<br>
16 . Search for packages

{% highlight terminal %}

$ apk search -v 'package_name'


# Example
$ apk search -v 'node'


{% endhighlight %}


<br>
17 . Install bash and switch current shell to bash

{% highlight terminal %}

$ apk add bash

$ bash

{% endhighlight %}


<br>
18 . View docker networks

{% highlight terminal %}

$ docker network list

{% endhighlight %}


<br>
19 . List running images

{% highlight terminal %}

$ docker images

{% endhighlight %}


<br>
20 . List all images

{% highlight terminal %}

$ docker images -a

{% endhighlight %}


<br>
21 . Delete image

{% highlight terminal %}

$ docker rmi image_id

# Example
$ docker rmi c430ys54229b

{% endhighlight %}


<br>
22 . Tag an image

{% highlight terminal %}

$ docker tag image_id tage_name

{% endhighlight %}


<br>
23 . Create an image from a container. (A snapshot of the container)

{% highlight terminal %}

$ docker commit container_id image_id

{% endhighlight %}


<br>
24 . Delete all images

{% highlight terminal %}

$ docker rmi $(docker images -q)

{% endhighlight %}


<br>
25 . Remove an image that is not used in a container

{% highlight terminal %}

$ docker image_id prune -a

{% endhighlight %}


<br>
26 . Clean an unused image

{% highlight terminal %}

$ docker image_id prune

{% endhighlight %}


<br>
27 . Prune entire system

{% highlight terminal %}

$ docker system prune

{% endhighlight %}


<br>
28 . Initialize the swarm mode and listen to a specific interface

{% highlight terminal %}

$ docker swarm init --advertise-addr 47.51.0.26

{% endhighlight %}


<br>
29 . Join an existing swarm as a manager node

{% highlight terminal %}

$ docker swarm join --token manager_token_here 47.51.0.26:2377

{% endhighlight %}


<br>
30 . Join a swarm as a worker node

{% highlight terminal %}

$ docker swarm join --token manager_token_here 47.51.0.26:2377

{% endhighlight %}


<br>
31 . List all nodes in the swarm

{% highlight terminal %}

$ docker node ls

{% endhighlight %}


<br>
32 . Create a service from an image on the existing port and deploy five instances

{% highlight terminal %}

$ docker service create --replicas 5 -p 80:80 name -webnginx

{% endhighlight %}


<br>
33 . List services running in a swarm

{% highlight terminal %}

$ docker service ls

{% endhighlight %}


<br>
34 . Scale a service

{% highlight terminal %}

$ docker service scale web=5

{% endhighlight %}


<br>
35 . List the tasks of a service

{% highlight terminal %}

$ docker service ps web

{% endhighlight %}


<br>
36 . View all running services

{% highlight terminal %}

$ docker stack services stack_name

{% endhighlight %}


<br>
37 . View all service logs

{% highlight terminal %}

$ docker service logs stack_name service_names

{% endhighlight %}


<br>
38 . Scale a service across qualified nodes

{% highlight terminal %}

$ docker service scale stack_name_service_name = replicas

{% endhighlight %}


<br>
39 . Leave a swarm

{% highlight terminal %}

$ docker stack rm stack_name

{% endhighlight %}


<br>
40 . Remove a swarm

{% highlight terminal %}

$ docker stack rm stack_name

{% endhighlight %}



<br>
*Thanks for reading, see you in the next one!*
