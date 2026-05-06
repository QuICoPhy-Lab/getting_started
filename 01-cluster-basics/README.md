# Cluster basics

This guide covers the minimum steps to connect, transfer files, and submit jobs on a cluster.

A computing cluster is a set of networked machines (nodes) that share storage and a job
scheduler. You log in to a *login node*, prepare your code and data there, then ask the
scheduler (SLURM, in our case) to run your work on *compute nodes* with the CPUs, GPUs,
memory, and time you requested.

Notable resources include the
[Digital Research Alliance of Canada Wiki](https://docs.alliancecan.ca/wiki/Getting_started),
[Digital Research Alliance of Canada User Portal](https://portal.alliancecan.ca/login) and
[Institut Quantique Computing Cluster](https://ccs-udes.github.io/hpc-iq/en/).

## 0. Available clusters

Two categories of clusters are available. IQ clusters are smaller and typically faster to
access for everyday work; Alliance clusters are larger national systems better suited to
heavier or longer jobs.

**Institut Quantique**:

- `iq-main` (shared with all IQ research groups)
- `iq-aphex` (shared with QuICoPhy Lab group)

**Digital Research Alliance of Canada**:

- `fir`
- `rorqual`
- `narval`
- `niagara`

## 1. Connect with SSH

SSH (Secure Shell) opens a remote terminal session on the cluster's login node. From there
you can edit files, move data, and submit jobs as if you were sitting in front of it.

For Alliance clusters, use:

```bash
ssh <username>@<cluster>.alliancecan.ca
```

For IQ infrastructure, use:

```bash
ssh <username>@hpc.iq.ccs.usherbrooke.ca
```

### SSH keys (recommended)

Passwordless SSH is strongly recommended. Instead of typing a password every time, your
local machine proves its identity with a cryptographic key pair: a *private key* that
stays on your laptop, and a *public key* registered on the cluster. Create the pair once
with `ssh-keygen` and copy the public key over.

A short setup guide is available in [`ssh/README.md`](./ssh/README.md), with a sample SSH config in [`ssh/config`](./ssh/config).

## 2. Go to your project directory

Your home directory has small quotas and is not meant for large datasets or many files.
Real work lives in the project space, which is larger and shared with your group.

- Alliance clusters: `~/projects/def-ko1/<username>/`
- IQ shared storage: `/net/nfs-iq/data/<username>/`

## 3. Upload project files

To run code on the cluster you need to get it there first. You can either:

1. Copy files directly with `scp` — quickest for one-off transfers from your laptop.
2. Clone your GitHub repository on the cluster — better for code you change often, since
   you can `git pull` updates instead of re-uploading.

Example with `scp`:

```bash
scp -r <local_folder> <username>@<cluster>.alliancecan.ca:~/projects/def-ko1/<username>/
```

## 4. Submit a first SLURM job

You don't run heavy code directly on the login node. Instead you write a small shell
script that declares the resources you need (CPUs, GPUs, memory, walltime) at the top,
then hand it to SLURM with `sbatch`. SLURM queues the job and runs it on a compute node
once resources are free.

Template scripts are available in [`scripts/`](./scripts):

- `base_script.sh`: simple single-job template.
- `advanced_script.sh`: array jobs + temporary environment creation.
- `advanced_script_gpus.sh`: GPU-enabled variant.

Submit with:

```bash
sbatch scripts/base_script.sh
```

Follow job status with your cluster tools (`sq`, `squeue`, etc.), then inspect output logs (`.out` files).
The `.out` file captures whatever your script wrote to standard output, so it's the first
place to look when a job behaves unexpectedly.

## 5. Download results

Once a job finishes, the same `scp` command works in reverse to copy outputs back to your
local machine:

```bash
scp -r <username>@<cluster>.alliancecan.ca:~/projects/def-ko1/<username>/<results_path> .
```

For many files, consider creating an archive first. Transferring one large file is much
faster than thousands of small ones, since each file pays a per-file overhead:

```bash
tar -czf results.tar.gz <results_folder>
```
