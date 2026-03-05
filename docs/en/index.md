# GPU queue management for experiments — gpuq

`gpuq` is a tool designed to manage shared GPU usage on a research server.

On many servers used by students and research groups, several people run machine learning experiments at the same time. This often leads to problems such as:

- multiple users trying to use the same GPU
- experiments failing because the GPU is already in use
- difficulty reproducing experiments across different environments

`gpuq` solves these problems by using:

- **Docker** to define the execution environment of experiments
- **docker-compose** to describe how each experiment is executed
- **a job queue** that guarantees only one experiment uses the GPU at a time

---

## What you will learn in this documentation

This documentation explains step by step how to:

1. Understand what Docker is and why it is used in experimentation
2. Define an experiment using `Dockerfile` and `docker-compose`
3. Run experiments on the server using `gpuq`

No previous Docker experience is required.

---

## Workflow

The typical workflow is the following:

1. Define the experiment environment using a **Dockerfile**
2. Define how the experiment runs using **docker-compose**
3. Submit the experiment to the queue using **gpuq**

---

## Experiment structure

In this documentation we will use a simple structure for experiments:

```plain
experiment/
├── Dockerfile
├── docker-compose.yml
├── train.py
├── requirements.txt
├── data/
└── results/
```

Where:

- **Dockerfile** defines the execution environment
- **docker-compose.yml** defines how the experiment runs
- **data/** contains the input data
- **results/** contains the generated results