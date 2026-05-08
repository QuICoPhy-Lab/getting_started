# Docker

This section covers building container images locally with Docker.

A container packages your code together with the operating system libraries, system
packages, and Python/conda environments it needs to run. The result is a single image
that behaves the same on your laptop and on the cluster, which removes most "works on
my machine" problems.

## Goal

Build and test a reproducible image of your software environment locally, so it can
later be shipped to the cluster and executed with Apptainer.

## Typical workflow

1. Write a `Dockerfile` that describes the base image, dependencies, and entry point.
2. Build the image locally: `docker build -t <name>:<tag> .`.
3. Run it interactively to verify your code works inside the container:
   `docker run --rm -it <name>:<tag>`.
4. Push the image to a registry (Docker Hub, GitHub Container Registry, etc.) so the
   cluster can pull it.

## Why Docker locally and not on the cluster

Docker requires root privileges, which shared HPC systems do not grant to users. You
therefore use Docker on your own machine to author and test images, then switch to
Apptainer on the cluster, which is designed to run as an unprivileged user.
