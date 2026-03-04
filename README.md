# GPUQ Documentation

This repository contains the documentation for **gpuq**, a GPU queue system designed to manage shared GPU resources for machine learning experiments.

The documentation explains how to:

- Run experiments using Docker
- Configure Docker Compose for GPU usage
- Use the `gpuq` system to submit jobs
- Follow best practices for reproducible experimentation

The documentation is written in **Markdown** and built using **MkDocs** with the **Material for MkDocs** theme.

---

## Building the documentation locally

Install the dependencies:

```bash
pip install -r requirements.txt
```

Run the local development server:

mkdocs serve

Then open:

http://127.0.0.1:8000

The site will automatically reload when files are modified.

---

## Deployment

The documentation is automatically deployed to GitHub Pages using a GitHub Actions workflow.

Every commit pushed to the main branch triggers a rebuild and deployment of the site.

---

## License

This documentation is licensed under the Creative Commons Attribution-ShareAlike 4.0 International License (CC BY-SA 4.0).

See LICENSE.md for details.
  