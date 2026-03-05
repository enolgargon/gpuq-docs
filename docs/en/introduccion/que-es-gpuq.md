# What is gpuq?

`gpuq` is a tool designed to manage shared GPU usage on a research server.

In many research groups, several people use the same server to run machine learning experiments. These experiments usually require GPU usage in order to train models in a reasonable amount of time.

When multiple users share the same server, it is common to encounter problems such as:

- several users trying to use the same GPU at the same time
- experiments failing because the GPU is already in use
- difficulty coordinating who can use the GPU at any given time

`gpuq` solves this problem by using **a job queue** that manages the execution of experiments.

---

## How gpuq works

The way `gpuq` works is simple.

Users **submit their experiments to a queue**, instead of executing them directly on the server.

`gpuq` runs the experiments **one at a time**, ensuring that only one experiment is using the GPU at any given moment.

The workflow is as follows:

1. The user defines their experiment using Docker
2. The user submits the experiment to the queue using `gpuq`
3. `gpuq` executes the experiments in the order they were submitted

This prevents multiple experiments from competing for the same GPU.

---

## Relationship between Docker and gpuq

On this server, experiments are executed using **Docker containers**.

This means that each experiment defines its execution environment using:

- a **Dockerfile**, which defines the experiment environment
- a **docker-compose.yml**, which describes how the experiment is executed

`gpuq` uses these files to launch the experiment inside a container.

This provides several advantages:

- each experiment runs in an isolated environment
- there are no dependency conflicts
- experiments are reproducible

---

## Complete workflow

The typical workflow for running an experiment on the server is the following:

1. Define the experiment environment with a **Dockerfile**
2. Define how the experiment runs with **docker-compose**
3. Submit the experiment to the queue using **gpuq**

In the following sections you will learn how to:

- write a **Dockerfile**
- configure a **docker-compose.yml**
- run experiments using **gpuq**