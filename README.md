# Docker tutorial

This tutorial will walk you through using Docker for assigned projects in CS 7610: Foundations of Distributed Systems.

## Why use Docker for CS 7610
Among other reasons using Docker for projects is a good idea, some reasons for using it for projects in CS 7610 are:

1. Project submissions become highly reproducible, thus less likely to be graded incorrectly
2. It will become easier to write and test out distributed application, which will likely run on multiple nodes, once you get past the initial learning curve of Docker


## Simple application
I'll walk you through writing a simple C "hello world" application in Docker. Later on in the tutorial, we'll get into networking in Docker.

Install Docker by following instructions on [Docker's website](https://docs.docker.com/install/).

Create a directory `docker-hello` and create the following two files in the directory: `hello.c` and `Dockerfile`
