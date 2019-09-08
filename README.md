# Docker tutorial

This tutorial will walk you through using Docker for assigned projects in CS 7610: Foundations of Distributed Systems.
## Docker Intro

## Why use Docker for CS 7610
Among other reasons using Docker is a good idea in general, some reasons for using it for projects in CS 7610 are:

1. Project submissions become highly reproducible, thus less likely to be graded incorrectly
2. It will become much easier to write and test out distributed applications for the course, which will likely have multiple instances running, once you get past the initial learning curve of Docker


## Simple application
I'll walk you through writing a simple C "hello world" application in Docker. Later on in the tutorial, we'll get into networking in Docker.

Install Docker by following instructions on [Docker's website](https://docs.docker.com/install/).

Create a directory `docker-hello` and create the following two files in the directory: `hello.c` and `Dockerfile`.

The `hello.c` file is a simple "hello world" application in C:
```
#include <stdio.h>

int main(){
	printf("hello world");
}

```

The `Dockerfile` will look something like this:
```
FROM ubuntu:latest

RUN apt-get update
RUN apt-get install -y gcc

ADD hello.c /app/
WORKDIR /app/
RUN gcc hello.c -o hello

ENTRYPOINT /app/hello
```

While there are many, many variations of how this Dockerfile could look like and still perform the same task, this is a good, simple one for the purpose.

While most of it is pretty self-explanatory, especially if you're comfortable using the shell, I'll walk through what the file means.

The first line, `FROM ubuntu:latest`, specifies what base image we're using for our container. In this you have many options, I usually go with `ubuntu:latest` and build on top of that.

Next we install gcc, which we'll use to compile `hello.c` (alternatively you could have used a base image that already included gcc). After that, we copy over the `hello.c` file into the image using the `ADD` directive.

Next, using the `WORKDIR` directive, we move to the `/app/` directory in the image and compile `hello.c` using gcc.

Finally, I set the entrypoint of our container to be the newly built `hello` binary. What this means is, as soon as the container runs, `/app/hello` will be excuted.

Let's now build the image and run the container. While in the `docker-hello` directory, run the following command in the shell:
```
docker build . -t "dockerhello"
```

This builds the container using the Dockerfile in the current directory, and gives it the tag "dockerhello". At this point, `hello.c` has been copied over and built using gcc inside our Docker image, and will execuse as soon as the container starts.

Now we run it:
```
docker run dockerhello
```
And you should see "hello world" printed on the screen.

## Networked application in Docker

In order to have our Docker containers talking to each other, we are going to create a user-defined bridge network in Docker. While containers can communicate over the default bridge network in Docker, the bridge does not provide container name to IP address resolution. This name resolution is useful to have because, while writing your distributed application, you can make a peer list of container names once and use it between different runs of your system without having to update it, as long as you're using the same container names between runs.

To start off, we list the existing Docker networks using:
```
docker network list
```

Next we're going to add a Docker network:
```
docker network create --driver bridge mynetwork
```


Now to create a Dockerfile that will let use play around with networking between containers:
```
FROM ubuntu:latest

RUN apt-get update
RUN apt-get install -y netcat iputils-ping
```

Put the above Dockerfile in a new directory called `docker-network`. We will be using `netcat`, the TCP/IP Swiss Army Knife, to test out networking between Docker containers. `netcat` comes in very handy when testing out networking applications and I'd strongly recommend getting comfortable with it.

Next, navigate to `docker-network` and build the Docker image:
```
docker build . -t dockernetwork
```

Now we're going to run two container instances of the image `dockernetwork` and have them talk to each other. These containers will be part of the `mynetwork` we previously created.

In a shell, run
```
docker run --name first --network mynetwork -it dockernetwork
```
In another shell, run:
```
docker run --name second --network mynetwork -it dockernetwork
```

