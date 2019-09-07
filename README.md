# Docker tutorial

This tutorial will walk you through using Docker for assigned projects in CS 7610: Foundations of Distributed Systems.

## Why use Docker for CS 7610
Among other reasons using Docker is a good idea in general, some reasons for using it for projects in CS 7610 are:

1. Project submissions become highly reproducible, thus less likely to be graded incorrectly
2. It will become much easier to write and test out distributed applications for the course, which will likely run on multiple instances running, once you get past the initial learning curve of Docker


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

ADD hello.c /app/
RUN apt-get update
RUN apt-get install -y gcc

WORKDIR /app/
RUN gcc hello.c -o hello

ENTRYPOINT /app/hello
```

While there are many, many variations of how this Dockerfile could look like and still perform the same task, this is a good, simple one for the purpose.

While most of it is pretty self-explanatory, especially if you're comfortable using the shell, I'll walk through what the file means.

