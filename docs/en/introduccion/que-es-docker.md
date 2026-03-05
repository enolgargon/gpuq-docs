# What is Docker?

Docker is a tool that allows programs to run inside **isolated environments called containers**.

A container includes everything required to run a program:

- the base operating system
- the required libraries
- Python dependencies
- the experiment code itself

This allows an experiment to be executed **always in the same environment**, regardless of the system where it runs.

---

## The problem it tries to solve

In machine learning experimentation it is very common to encounter problems such as:

- an experiment works on your computer but fails on the server
- different versions of Python or a library cause errors
- installing dependencies breaks other projects
- different users require different environments

For example, one experiment may require Python 3.10 and PyTorch 2.1, while another may need Python 3.8 and TensorFlow 2.10.

Installing all of this in the same system often creates conflicts.

---

## The solution: containers

Docker allows each experiment to be encapsulated in its own environment.

This environment is described in a file called a **Dockerfile**, which defines:

- which base image is used
- which dependencies are installed
- which command runs the experiment

When the experiment is executed, Docker creates a **container** with that environment.

This means that:

- each experiment has its own dependencies
- there are no conflicts between projects
- the experiment can be easily reproduced

---

## Advantages of using Docker in experimentation

Using Docker for experiments provides several important advantages.

### Reproducibility

The experiment environment is fully defined.

If someone else runs the same container, they will obtain **exactly the same environment**.

---

### Isolation

Each experiment runs in its own container, which means:

- it does not affect the system
- it does not break other experiments
- it does not require installing libraries globally

---

### Portability

A Docker container can run on any system where Docker is installed.

This allows experiments to be easily moved between:

- personal computers
- research servers
- computing infrastructures

---

## Docker on this server

On this server we use Docker to run all experiments.

Each experiment is defined using:

- a **Dockerfile** that describes the environment
- a **docker-compose.yml** that describes how it runs

These files allow the experiment to be fully defined.

However, there is still one problem.

If multiple users run containers that use the GPU at the same time, conflicts may occur.

To solve this we use **gpuq**, a tool that allows experiments to be **queued and executed in a controlled way**.