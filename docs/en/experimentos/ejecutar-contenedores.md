# Running Docker Containers

Once the **Dockerfile** and the **docker-compose** file have been defined, the next step is to run the experiment.

This section describes the basic Docker commands used to execute and manage containers on the server.

It is not necessary to know Docker in depth. We will only cover the most common commands used when working with experiments.

---

# Running an experiment

To run an experiment defined with `docker-compose`, use the command:

```bash
docker compose up
```

This command performs several actions:

1. Builds the image defined in the `Dockerfile` (if it does not already exist).
2. Creates the container.
3. Runs the experiment inside the container.

If the experiment has been executed previously and the image already exists, Docker will reuse that image to start the container.

---

## Selecting the docker-compose file

By default, Docker looks for a file named: `docker-compose.yml`.

If the project uses this name, **it is not necessary to specify anything else**.

However, if the project contains multiple `docker-compose` files, it is possible to select a specific one using the `-f` option.

For example:

```bash
docker compose -f docker-compose.test.yml up
```

This allows multiple execution configurations within the same project, for example:

- one for running the full experiment
- another for running tests or quick experiments

---

## Running in the background

Many experiments can take hours or even days.

To avoid keeping the terminal open for the entire time, you can use the `-d` option (*detached mode*):

```bash
docker compose up -d
```

or, if you want to specify the file:

```bash
docker compose -f docker-compose.yml up -d
```

In this mode, the container runs **in the background**, and you can close the terminal session and come back later.

---

# Stopping an experiment

To stop the containers created by `docker-compose`, use:

```bash
docker compose down
```

This command:

- stops the containers
- removes the containers created by `docker-compose`

It is important to note that **the Docker image is not removed**, only the container.

This means the experiment can be run again quickly without rebuilding the image.

---

# Viewing containers

To see the containers currently running, use:

```bash
docker container ls
```

This command displays information such as:

- container identifier
- image used
- status
- container name
- running time

---

## Viewing all containers

By default, only running containers are shown.

To see **all containers**, including those that have already finished, use the `-a` option:

```bash
docker container ls -a
```

This allows you to check whether an experiment has finished or ended with an error.

---

## Viewing the last container

The `-l` option shows **the most recently created container**:

```bash
docker container ls -l
```

This is useful when you have just run an experiment and want to quickly locate the corresponding container.

---

# Viewing container logs

To view the output of a container, use:

```bash
docker logs <container_id>
```

This command displays the standard output of the program running inside the container.

In machine learning experiments this usually includes:

- training progress
- metrics
- error messages
- debugging information

---

## Container identifier

The container identifier is usually a long string, for example `8f3a1c2b4d9e7c...`.
It is not necessary to type the full identifier.

Docker allows you to use **only the first characters**, as long as they are sufficient to uniquely identify the container.

For example:

```bash
docker logs 8f3a
```

This will work as long as no other container identifier begins with `8f3a`.

---

# Managing Docker images

The Docker images built on the system can be listed with:

```bash
docker image ls
```

This command shows:

- image name
- identifier
- creation date
- size

Images contain the complete environment required to run the experiment.

---

## Removing an image

To remove an image, use:

```bash
docker image rm <image_id>
```

Removing an image is **important and necessary when changes are made to the experiment code or environment**.

If an image already exists, `docker compose` will reuse it in subsequent runs and **will not automatically rebuild the image**.

This means that changes made to the code may not be reflected in the container.

Deleting the image forces Docker to rebuild it the next time the experiment is executed.

---

# Summary

The most commonly used commands when working with containers are:

| Command | Description |
|---|---|
| `docker compose up -d` | Run an experiment in the background |
| `docker compose down` | Stop and remove containers |
| `docker container ls` | Show running containers |
| `docker container ls -a` | Show all containers |
| `docker container ls -l` | Show the most recently created container |
| `docker logs <container>` | Show the output of a container |
| `docker image ls` | List available images |
| `docker image rm <image>` | Remove an image |