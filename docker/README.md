# Docker

## Standards

- Define image and container
- Run a container in the foreground, mapping ports
- List images and containers
- Show running containers
- Pull and push images to quay.io
- Create a Dockerfile containing `nginx` and build it

## Lesson

Starting at [the documentation](https://www.docker.com/resources/what-container) we get an idea of what Docker is:

> A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

The `container` concept is borrowed from shipping containers, which define a standard to ship goods globally. [Containerization](https://en.wikipedia.org/wiki/Containerization) dates back to early 18th Century coal mines. However, it wasn't until the 20th Century that _standardized_ containers became the norm. Docker does this in a modern way:

> Docker defines a standardized way to ship software.

Have you ever said or heard `It works on my machine!`? Docker lets you ship "your machine" to production by defining the runtime environment for the software and isolating that environment from everything else running on the physical hardware.

Similar to code, developers typically share Docker _images_, which are then run as _containers_. The location that we will store our Docker images is [Quay.io](https://quay.io/repository/).

There are a few key terms for working with Docker. Per the [Glossary](https://docs.docker.com/glossary):

- **Image** - The basis for containers. An image:

  - contains a union of layered filesystems stacked on top of each other
  - does not have state, and it never changes
  - that has no parent is a `base image`

- **Container** - a runtime instance of a `docker image`

  - Consists of
    - Docker image
    - execution environment
    - standard set of instructions

- **Registry** - a delivery and storage system for named Docker images.
  - Use `docker push` and `docker pull` to push and pull images from the registry
  - Companies usually have private docker registries
  - Similar to `maven`, `npm`, and `artifactory`
  - We will use [Quay.io](https://quay.io/) as our Docker registry

* **Tag** - a label applied to a Docker image in a repository.
  - How various images in a repository are distinguished from each other.
  - Can be almost anything you want
  - It is best to give it a meaningful, human-readable name that can be easily referenced later.

Seeing an example helps.

We begin with a classic [Hello, World!](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program) example.

Docker offers a lot, including the ability to download and run publicly available images. The most common spot for image hosting is [Dockerhub](https://hub.docker.com/).

We can easily see what Docker offers using `--help`:

```shell
$:> docker --help
```

We use `docker run` to start our first Docker container:

```shell
$:> docker run hello-world
```

The `-p 8080:8080` indicates a port mapping from `localhost` to container host, and `hello-world` is the name of the image on [Dockerhub](https://hub.docker.com/_/hello-world) that we want to run as a container.

It is worth reading the output carefully as it details the process Docker uses:

```shell
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

Following Docker's output, we try a slightly more ambitious example:

```shell
$:> docker run -it ubuntu bash
```

the flag `-i` indicates that we want an interactive session (i.e. keep the container running), and `-t` indicates [TTY](https://stackoverflow.com/questions/22272401/what-does-it-mean-to-attach-a-tty-std-in-out-to-dockers-or-lxc), `ubuntu` is the image name on Dockerhub and `bash` gives a Bash shell.

This image is not particular interesting though since it has only the base software of Ubuntu installed.

#### Creating a Docker Image

What if we want to containerize something we created and publish it on Quay.io (or Dockerhub)? What would that require? A `Dockerfile`.

A `Dockerfile` is a ["text document that contains all the commands you would normally execute manually in order to build a Docker image."](https://docs.docker.com/glossary/). An example helps clarify this.

Create in a new folder in `~/workspace/` named `dockerfile-example`, cd into `dockerfile-example`. Next, `touch Dockerfile && code .`

We will populate the `Dockerfile` with:

```dockerfile
FROM alpine

CMD ["echo", "Hello, world! This is a simple Dockerfile using echo."]
```

In order to build the image we begin with:

```shell
$:> docker build --no-cache -t simple-dockerfile-example .
$:> docker run simple-dockerfile-example
A simple Dockerfile
```

While this is helpful for getting the basic workflow of `docker build` / `docker run` it is more instructive to see an application be built. For this example, we will adapt from the [Node with Docker Official Guide](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/).

To demonstrate how fast it is to go from project to containerized application we begin with a new Express app:

```shell
$:> npx express-generator app-to-containerize
$:> cd app-to-containerize && npm install
```

inside of `views/index.jade` we add a single line:

```
extends layout

block content
  h1= title
  p Welcome to #{title}
  p This is a Node Application that has been containerized with Docker!
```

next, we create a Dockerfile with `touch Dockerfile`. First, let's use Google or Dockerhub to see if there are any existing images we can use as a _base image_ for our build.

The [Official NodeJS](https://hub.docker.com/_/node) image should work:

```dockerfile
// Use node Docker image, version 16-alpine
FROM node:16-alpine

// From the documentation, "The WORKDIR instruction sets the working directory (folder) for any
// RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile"
WORKDIR /usr/src/app

// COPY package.json and package-lock.json into root of WORKDIR
COPY package*.json ./

// Executes commands
RUN npm install

// Copies files from source to destination, in this case the root of the build context
// into the root of the WORKDIR
COPY . .

// Documents that the container process listens on port 3000
EXPOSE 3000

// Command to use for starting the application
CMD ["npm", "start"]
```

One important concept in the above example is the [build context](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#understand-build-context). In the simplest terms possible, the build context is the folder where we run `docker` commands. Our `Dockerfile` will typically be located at the same level of the file tree as the build context.

Since our build context, for this example, includes a folder of installed dependencies, `node_modules/`, we can use `.dockerignore` to ensure they are not copied into the image:

```docker
node_modules/
```

Next we build the image and start a container with this image:

```shell
$:> docker build --no-cache -t express-example-app .
$:> docker run --rm  -p 127.0.0.1:8080:3000 express-example-app
```

If we visit `http://localhost:8080` in a browser, then we see the sample express application.

A few useful commands for examining the state of the Docker container:

- `docker ps -a` to list all docker containers
- `docker images` to list all images installed
- `docker logs <container-id>` - shows the logs for a given container, with `-f` flag (similar to `tail -f`) waits for logs to be appended to
- `docker rm $(docker ps -qa)` to remove all stopped containers (compare output of `docker ps -qa` and `docker ps -a`)

#### Environment Variables

_Question:_ If the Docker container is an independent runtime, then how do we inject environment-based values?

One principle of [12-factor application](https://12factor.net/) development is [config](https://12factor.net/config) or _store config in the environment_. A real world example of why this is important is that we do not want to use something like a database between development, test, and production. _Why?_ Because it is easy to make mistakes with no environment isolation, especially if the configuration is hard coded into the codebase and conditionally applied.

A small example helps demonstrate this concept.

1. `cd ~/workspace`
1. `mkdir docker-lister && cd docker-lister`.
1. Create a file named `lister.sh` that contains the following Bash script that shows the current folder, contents of the folder, and value of environment variables:

```shell
#! /usr/bin/env sh

echo The current working directory is:
echo
pwd
echo
echo ------------------------------------

echo Here are the contents of the working directory:
echo
ls
echo
echo ------------------------------------

echo Here are the environment variables:
echo
env
echo
echo ------------------------------------
```

1. To test this locally, make `lister.sh` executable with `chmod +x ./lister.sh`
1. Run `./lister.sh` and review the output
1. Containerize the script using Docker, first `touch Dockerfile`
1. Our Dockerfile will once again use `alpine` for simplicity:

```Dockerfile
FROM alpine

RUN mkdir app
WORKDIR app

COPY . .

CMD ["./lister.sh"]
```

1. `docker build -t ibm/lister .` (**Warning:** Do not use your root folder, `/`, as the PATH as it causes the build to transfer the entire contents of your hard drive to the Docker daemon.)
1. `docker run ibm/lister` - compare this output to `./lister.sh`'s output
1. `docker run -e API_URL=https://thecatapi.com/ ibm/lister` - compare this output to the others

The `-e` flag allowed us to inject the pair `API_URL` / `https://thecatapi.com/` into the container's environment.

`lister.sh` demonstrates that **the Docker container is a self-contained environment**. In each instance we see the working directory (folder), its contents, and the value of environment variables depends on where `lister.sh` is running.

# Glossary

> Build once, run everywhere.

| Term      | Description                                                                                                                                                                                              |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Container | Runtime instance of a docker image. Standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.     |
| Docker    | Defines a standardized way to ship software (using containers).                                                                                                                                          |
| Image     | Union of layered filesystems. Lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings. |
| Registry  | Delivery and storage system for named Docker images.                                                                                                                                                     |
| Tag       | Label applied to a Docker image in a repository.                                                                                                                                                         |
