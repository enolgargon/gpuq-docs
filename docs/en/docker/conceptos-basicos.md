# Basic Docker Concepts

This section introduces some basic Docker concepts that are necessary to understand how experiments are executed on this server.

It is not necessary to know Docker in depth. We will only cover the minimum concepts that will be used in the examples throughout this documentation.

---

## Image

A **Docker image** is a template that contains everything required to run a program.

An image can include:

- a base operating system
- system libraries
- Python dependencies
- tools required for the experiment

Images are used as the base for creating containers.

---

## Container

A **container** is a running instance of an image.

We can think about the relationship between images and containers in a similar way to:

- image → template
- container → running program

When we run an experiment with Docker, what actually happens is that Docker creates a container from an image and executes the program inside it.

Each container is **isolated from the system and from other containers**.

This allows different experiments to use different dependencies without interfering with each other.

---

## Dockerfile

A **Dockerfile** is a file that describes how to build a Docker image.

It defines aspects such as:

- the base image
- the dependencies that must be installed
- the working directory
- the command that runs the experiment

A simple example of a Dockerfile could be:

```dockerfile
FROM python:3.10

WORKDIR /workspace

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY train.py .

CMD ["python", "train.py"]
```

This file tells Docker how to build the image that will be used to execute the experiment.

In the next section we will look in more detail at how to write a Dockerfile.

---

## Volumes

Volumes allow files to be shared between the host system (the server) and the container.

This is especially useful when working with:

- datasets
- experiment results
- configuration files

For example, we can mount a directory from the server inside the container: `./data → /data`

In this way the container can read the data from `/data`, and the results written by the container remain on the server.

This allows us to clearly separate:

- the experiment environment (inside the container)
- the data and results (outside the container)

---

## Environment Variables

Environment variables allow parameters to be passed to the experiment when the container is executed.

For example, we can define parameters such as:

```plain
EPOCHS=50
LEARNING_RATE=0.001
```

The program can read these variables to configure its behavior.

This allows experiment parameters to be changed without modifying the code.