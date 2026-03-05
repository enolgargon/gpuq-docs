# Minimal Example

This section shows a minimal example of how to run an experiment using the files described in the previous sections.

A typical project may have a structure like the following:

```plain
experiment/
├── Dockerfile
├── docker-compose.yml
├── docker-compose.test.yml
├── requirements.txt
├── main.py
├── model.py
├── test.py
├── data/
└── results/
```

The experiment can be executed in two different ways depending on whether it requires a GPU or not.

- Experiments **without GPU** → run directly with `docker compose`
- Experiments **with GPU** → submit to the queue with `gpuq`

---

# Running an experiment without GPU

If the experiment does not require a GPU, it can be executed directly with Docker.

For example, to run the experiment defined in `docker-compose.yml`:

```bash
docker compose up -d
```

This command:

1. builds the image defined in the `Dockerfile` (if it does not exist)
2. creates the container
3. runs the experiment in the background

---

# Running an experiment with GPU

Experiments that use a GPU **must not be executed directly with Docker**.

Instead, they must be submitted to the queue using **gpuq**.

This guarantees that:

- only one experiment uses the GPU at a time
- experiments are executed in order
- a persistent record of executed jobs is maintained

---

## Submitting a job to the queue

To submit an experiment to the queue, use the command:

```bash
gpuq submit --project PATH
```

where `PATH` is the directory containing the project.

For example:

```bash
gpuq submit --project ./experiment-name
```

This command:

- registers the job in the queue
- generates a unique identifier for the job
- associates the job with the user who submitted it

The job remains in the **queued** state until the system executes it.

---

## Selecting the docker-compose file

By default, `gpuq` will use the file `docker-compose.yml`.

If you want to use another `docker-compose` file, you can specify it using the `--compose` option.

For example:

```bash
gpuq submit --project ./experiment --compose docker-compose.test.yml
```

---

# Listing jobs in the queue

To see the jobs registered in the queue, use:

```bash
gpuq list
```

This command shows information such as:

- job identifier
- user who submitted it
- creation date
- description

---

# Cancelling a job

To cancel a job, use:

```bash
gpuq cancel JOB_ID
```

For example:

```bash
gpuq cancel job-1a2b3c4d
```

If the job is in the **queued** state, it will be moved to the **canceled** state.

If the job is in the **running** state, the system will mark the job as canceled and the execution process will take care of stopping it.