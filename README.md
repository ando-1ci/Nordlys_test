# Managing Application Images with Kubernetes

## Contents

- [Overview](#overview)
- [Before you begin](#before-you-begin)
- [Setting up directories](#setting-up-directories)
  - [Directory structure](#directory-structure)
  - [Creating directories](#creating-directories)
- [Adding an application](#adding-an-application)
- [Adding dependencies](#adding-dependencies)
- [Creating a Dockerfile](#creating-a-dockerfile)
- [Building an image](#building-an-image)
- [Related topics](#related-topics)

## Overview<a id="overview"></a>

With Kubernetes, you can automate deployment, scaling, and management of containerized applications. This article shows how to prepare an application image, set up the environment, and to deploy the application on a Kubernetes cluster.

For your convenience, all steps are illustrated with the examples from a simple Kubernetes test project.

## Before you begin<a id="before-you-begin"></a>

Install Docker to your machine. To do so, open the [link](https://docs.docker.com/get-docker), select the operating system, and follow the instructions.

## Setting up directories<a id="setting-up-directories"></a>

### Directory structure<a id="directory-structure"></a>

Please ensure that your Kubernetes project complies with the following directory structure convention adopted by Nordlys:

|Path                            |Directory content              |
|--------------------------------|-------------------------------|
|&lt;rootDir&gt;                 |The whole project.<br>**Note**: Make the root directory name descriptive and meaningful.|
|&lt;rootDir&gt;/application     |The application files.         |
|&lt;rootDir&gt;/dependencies    |OS, libraries, and other dependencies.|
|&lt;rootDir&gt;/context         |Context and a Dockerfile.      |

### Creating directories<a id="creating-directories"></a>

To create a directory, run **mkdir**. For example, if the test project's root directory name is **rootDir**:

```bash
mkdir rootDir
mkdir rootDir/application
mkdir rootDir/dependencies
mkdir rootDir/dockerfile
```

## Adding an application<a id="adding-an-application"></a>

Copy application files to the **&lt;rootDir&gt;/application** directory.

**Example**. The test application is the **application.py** file with the following code:

```python
import http.server
import socketserver

PORT = 8000

Handler = http.server.SimpleHTTPRequestHandler

httpd = socketserver.TCPServer(("", PORT), Handler)

print("serving at port", PORT)
httpd.serve_forever()
```

## Adding dependencies<a id="adding-dependencies"></a>

The set of dependencies vary for different applications. You can find the most popular dependencies in [Docker Hub](https://hub.docker.com/). Download the required dependencies and copy them to **&lt;rootDir&gt;/dependencies**.

**Example**. The test project's **dependencies** folder contains the images of [Ubuntu](https://hub.docker.com/_/ubuntu) and [Python](https://hub.docker.com/_/python).

## Creating a Dockerfile<a id="creating-a-dockerfile"></a>

A Dockerfile is an extensionless text-based file that contains a set of instructions to create a container image. For more details, see [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/).

In **&lt;rootDir&gt;/dockerfile**, create a file named **Dockerfile**.

**Example**. The test project Dockerfile:

```python
# Use base image from the registry
FROM python:3.5

# Set the working directory to /app
WORKDIR /app

# Copy the 'application' directory contents into the container at /app
COPY ./application /app

# Make port 8000 available to the world outside this container
EXPOSE 8000

# Execute 'python /app/application.py' when container launches
CMD ["python", "/app/application.py"]
```

## Building an image<a id="building-an-image"></a>

To build an container image, run **docker build**, which has the following syntax:

```bash
$ docker build [OPTIONS] PATH
```

Options:

- Use the **-f** flag to specify the path to a Dockerfile.
- Use the **-t** flag to tag the image. For more details about valid tags, see [docker tag](https://docs.docker.com/engine/reference/commandline/tag/).

**Example**:

```bash
$ docker build -f PATH/rootDir/context/Dockerfile -t testProject
```

For more details on how to automatically build images using a Dockerfile, see [Dockerfile reference](https://docs.docker.com/engine/reference/builder/).

Now, you can export the application image to a repository.

## Related topics<a id="related-topics"></a>

- [Docker tag](https://docs.docker.com/engine/reference/commandline/tag/)
- [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
- [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
