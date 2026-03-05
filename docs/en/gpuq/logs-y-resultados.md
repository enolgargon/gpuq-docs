# Logs and results

Once a job has been submitted to the queue and begins execution, it is common to want to check:

- the experiment output
- possible errors during execution
- the generated results

This section explains how to access this information.

---

# Experiment output

Experiments submitted through `gpuq` run inside Docker containers using the configuration defined in the `docker-compose` file.

For this reason, the experiment output can be inspected using standard Docker tools.

---

## Viewing container logs

To view the output of a container, use the command:

```bash
docker logs CONTAINER_ID
```

This command displays the standard output of the program running inside the container.

In machine learning experiments this usually includes:

- training progress
- model metrics
- debugging messages
- execution errors

---

## Locating the container

To find the container associated with an experiment you can use:

```bash
docker container ls -a
```

---

# Experiment results

In the recommended project structure, a directory called `results` is used. This directory is mounted inside the container through a volume defined in `docker-compose`.

This means that any file written by the experiment to `/results` inside the container will automatically appear in the project's `results/` directory.

This ensures that:

- results remain on the system even after the container finishes
- results can be analyzed without needing to access the container

---

# Debugging errors

If an experiment fails, the typical steps to investigate the problem are:

1. check the job status with `gpuq list`
2. locate the corresponding container
3. inspect the container logs with `docker logs`

Error messages from the program usually appear in the container logs.

---

# Cleaning up containers and images

It is important to remember that `gpuq` **does not remove Docker containers or images**.

When an experiment finishes:

- the container created by Docker remains on the system
- the Docker image used to run it also remains

Removing containers or images that are no longer needed is the responsibility of the user.

The commands required to manage these resources are described in the Docker section.

---

# Summary

The main tools used to inspect the execution of an experiment are:

| Command | Description |
|---|---|
| `gpuq list` | Show the status of jobs in the queue |
| `docker container ls` | Show running containers |
| `docker container ls -a` | Show all containers |
| `docker logs CONTAINER_ID` | View the output of a container |

Experiment results are stored in the `results/` directory of the project.