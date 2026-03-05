# Running jobs with gpuq

`gpuq` is a tool designed to submit GPU-based experiments to an execution queue on a shared server.

Instead of running a container directly with Docker, experiments that require a GPU must be submitted to the queue using `gpuq`. This makes it possible to control GPU usage and prevent multiple experiments from attempting to use it at the same time.

Each job submitted to the queue is recorded persistently in the filesystem.

---

# Submitting a job to the queue

To submit an experiment to the queue, use the command:

```bash
gpuq submit --project PATH
```

where `PATH` is the directory that contains the project.

For example:

```bash
gpuq submit --project ./experiment
```

The project directory must contain at least one `docker-compose` file that describes how to execute the experiment.

When this command is executed:

- a unique identifier is generated for the job
- the job is associated with the user who submitted it
- the creation timestamp is recorded
- the job is stored in the queue with state **queued**

The job will remain in that state until the system responsible for executing the queue starts it.

---

# Selecting the docker-compose file

By default, `gpuq` will use the `docker-compose.yml` file.  
If the project contains multiple `docker-compose` files, it is possible to select a specific one using the `--compose` option.

For example:

```bash
gpuq submit --project ./experiment --compose docker-compose.test.yml
```

This allows different configurations to be used within the same project.

For example:

- run the main experiment
- run quick tests
- run alternative configurations

---

# Adding a description to the job

It is possible to add an optional description to the job using the `--description` option.

For example:

```bash
gpuq submit --project ./experiment --description "baseline training"
```

The description is free text that helps identify the purpose of the experiment.

---

# What happens when a job is submitted

When `gpuq submit` is executed, the tool **does not run the experiment directly**.

Instead, it:

1. validates that the project directory exists
2. registers the job in the queue
3. assigns a unique identifier to the job
4. stores the job metadata in the filesystem

The job is registered with state `queued`.  
An independent system component will later execute the job.

---

# Job identifier

Each job receives a unique identifier with a format similar to `job-1a2b3c4d`.  
This identifier is used to:

- check the job status
- cancel the job
- identify the experiment in the queue

---

# What gpuq does and what it does not do

It is important to understand which responsibilities belong to `gpuq` and which do not.

`gpuq` is responsible for:

- registering jobs in a persistent queue
- assigning unique identifiers to jobs
- maintaining the state of each job
- allowing jobs to be listed and canceled

However, `gpuq` **does not execute containers directly**.  
The actual execution is performed by a separate system component.

In addition, `gpuq` **does not remove Docker containers or images**.

When a job finishes, the containers created by Docker remain in the system. Similarly, the Docker images built to execute the experiments are not automatically removed.

Cleaning up containers or images that are no longer needed remains the responsibility of the user.

The commands required to manage containers and images are described in the Docker section.

---

# Summary

The main commands used to submit jobs to the queue with `gpuq` are:

| Command                                                                | Description                                                                 |
|------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| `gpuq submit --project ./experiment`                                   | Submit an experiment to the queue using the project's `docker-compose.yml`. |
| `gpuq submit --project ./experiment --compose docker-compose.test.yml` | Submit the experiment using a specific `docker-compose` file.               |
| `gpuq submit --project ./experiment --description "baseline training"` | Submit an experiment with a description to identify the job.                |

In all cases the command registers the job in the queue, generates a unique identifier, and leaves the job in the `queued` state until the system responsible for executing the queue starts it.

In the next section we will see how to monitor jobs in the queue and check their status.