# Docker and Apptainer

This section is for container-based workflows on HPC systems.

## Goal

Use containers to make your environment reproducible across your local machine and the cluster.

## Typical workflow

1. Build and test your image locally (Docker).
2. Convert or run it on the cluster (Apptainer/Singularity).
3. Launch jobs using your container in SLURM scripts.
