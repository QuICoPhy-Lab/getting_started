# Apptainer

This section covers running containers on the cluster with Apptainer (formerly
Singularity).

Apptainer is the HPC-friendly container runtime: it does not require root, it integrates
cleanly with SLURM, and it can directly consume images built with Docker. Most clusters
expose it as a module (`module load apptainer`) or have it available by default.

## Goal

Take an image you built and tested locally with Docker, and execute it on a compute
node as part of a SLURM job.

## Typical workflow

1. Pull an image from a registry into a `.sif` file:
   `apptainer pull myimage.sif docker://<user>/<image>:<tag>`.
2. Run a command inside the container:
   `apptainer exec myimage.sif python my_script.py`.
3. Bind-mount your project directory so the container can see your data and write
   results back: `apptainer exec --bind $SCRATCH:/data myimage.sif ...`.
4. Wrap the call in a SLURM script and submit with `sbatch`.

## Notes

- `.sif` files are single-file images. Store them in your project space, not your home
  directory, since they can be large.
- Inside the container, your user identity and home directory are preserved by default,
  which is convenient but means writes can leak outside the container unless you bind
  explicit paths.
