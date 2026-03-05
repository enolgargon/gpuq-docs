# Creating a Dockerfile

A **Dockerfile** is a file that describes how to build the Docker image that will be used to execute an experiment.

This file defines:

- the base image
- the required dependencies
- the code that will be executed
- the command that launches the experiment

From the Dockerfile, Docker builds an **image** that will later be used to create the container where the experiment will run.

---

## Basic structure of a Dockerfile

A typical Dockerfile for an experiment may look like this:

```dockerfile
FROM pytorch/pytorch:2.2.0-cuda12.1-cudnn8-runtime

WORKDIR /workspace

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY train.py .

CMD ["python", "train.py"]
```

This file defines the environment required to execute the experiment.

---

## Base image

The `FROM` instruction specifies which image will be used as the base.

```dockerfile
FROM pytorch/pytorch:2.2.0-cuda12.1-cudnn8-runtime
```

Base images usually include:

- Python
- machine learning libraries

Choosing a good base image avoids having to install many dependencies manually.

---

## Working directory

The `WORKDIR` instruction defines the directory from which commands will be executed inside the container.

```dockerfile
WORKDIR /workspace
```

From this point onward, files will be copied and commands will be executed in that directory.

---

## Installing dependencies

Python dependencies are usually defined in a `requirements.txt` file.

```dockerfile
COPY requirements.txt .
RUN pip install -r requirements.txt
```

First the file is copied into the container and then the dependencies are installed.

This ensures that the experiment environment is fully defined.

---

## Copying the code

Next, the experiment code is copied.

```dockerfile
COPY train.py .
```

This makes the file available inside the container.

---

## Execution command

The `CMD` instruction defines which command will be executed when the container starts.

```dockerfile
CMD ["python", "train.py"]
```

In this case, the container will execute the `train.py` script.

---

## Best practices

### Avoid `COPY . .`

It is common to find Dockerfiles that contain:

```dockerfile
COPY . .
```

This copies the entire project directory into the container.

In experimentation this is usually a bad practice because it may include:

- datasets
- experiment results
- unnecessary files

It is preferable to copy **only the files required to run the experiment**.

---

### Separate code and data

Datasets and results should not be copied into the Docker image.

Instead, **volumes** should be used to mount those directories when the container is executed.

This allows you to:

- reuse the same image for different datasets
- avoid very large Docker images
- keep code and data separated

We will see how to do this in the `docker-compose` section.

---

### Use environment variables for configuration

In many experiments it is necessary to change parameters such as:

- number of epochs
- learning rate
- batch size
- input or output paths

A common bad practice is modifying the code every time a parameter needs to change and rebuilding the Docker image.

This requires rebuilding the image each time, which can be slow and unnecessary.

Instead, it is recommended to use **environment variables** to define these parameters.

For example, the experiment code can read values such as:

- EPOCHS
- LEARNING_RATE
- BATCH_SIZE

These variables can be defined when the container is executed.

For example:

```plain
EPOCHS=50
LEARNING_RATE=0.001
```

In this way:

- the experiment code does not change
- rebuilding the image is not necessary
- multiple experiments can be launched with different configurations

This allows the same Docker image to be reused for different experiment configurations.