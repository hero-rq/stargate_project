---

# **HPC Data Sourcing & Processing: Practical Guide**

## Table of Contents
1. [Quick HPC Context](#1-quick-hpc-context)  
2. [Preparing the HPC Environment](#2-preparing-the-hpc-environment)  
3. [Getting Data Onto HPC](#3-getting-data-onto-hpc)  
   - 3.1 [Local Machine Upload via `scp` or SFTP](#31-local-machine-upload-via-scp-or-sftp)  
   - 3.2 [Cloning a GitHub Repository](#32-cloning-a-github-repository)  
   - 3.3 [Other Sources & Workarounds](#33-other-sources--workarounds)  
4. [Managing Data in HPC Directories](#4-managing-data-in-hpc-directories)  
5. [Practical Example: Multi-Source Data Processing on HPC](#5-practical-example-multi-source-data-processing-on-hpc)  
   - 5.1 [Overview of the Example](#51-overview-of-the-example)  
   - 5.2 [Step-by-Step Demo](#52-step-by-step-demo)  
6. [Wrapping Up](#6-wrapping-up)

---

## 1. Quick HPC Context

Unlike Google Colab or Kaggle (where data can be fetched with Python’s `requests` or Kaggle APIs directly from the web), HPCs are often **behind firewalls**, sometimes **lack direct internet access**, or enforce usage of [SSH, scp, or specific modules](https://docs.hpc.shef.ac.uk/). That means **you have to plan** how to move data onto the cluster:

- **(Local → HPC)** You might upload from your laptop/desktop.  
- **(GitHub → HPC)** You might clone or download code/data if that’s allowed.  
- **(HPC scratch areas)** HPC typically provides a “fast scratch” or “parallel scratch” location for large data sets.

---

## 2. Preparing the HPC Environment

1. **Log into HPC** (via SSH, e.g., `ssh <username>@stanage.shef.ac.uk`).  
2. **Get an interactive session** (example):
   ```bash
   srun --pty bash -i
   ```
   This launches an interactive shell on a compute node.

3. **Load your modules** (Java, Anaconda, etc.):
   ```bash
   module load Java/17.0.4
   module load Anaconda3/2024.02-1
   source activate myspark
   export LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8
   ```
   - Spark depends on Java.  
   - `myspark` is our Python conda environment with `pyspark==3.5.4`.

---

## 3. Getting Data Onto HPC

### 3.1 Local Machine Upload via `scp` or SFTP

1. **SCP (Command-line)**  
   ```bash
   # Example: from your local terminal
   scp /path/to/local/data.csv \
       <username>@stanage.shef.ac.uk:/users/my_auth/com6012/ScalableML/Data
   ```
   - Adjust paths accordingly.  
   - This copies `data.csv` from your local machine into HPC folder `myproject/data`.

2. **SFTP Clients**  
   - **FileZilla** or **WinSCP** let you do drag-and-drop.  
   - MobaXterm also has an SFTP side panel (though HPC sometimes restricts it).  
   - Great for **smaller** or single-file uploads.

**Why?** HPC typically blocks inbound HTTP/HTTPS so you can’t just `wget` from random websites. Instead, you download locally, then `scp` or SFTP it into HPC.

---

### 3.2 Cloning a GitHub Repository
(recommended in '/users/my_auth/com6012/ScalableML/Data' directory)

If your HPC environment **does allow** outbound internet access for GitHub (some HPCs do, others don’t), you can do:

```bash
git clone https://github.com/YourAccount/DataRepo.git
```
**Pitfalls:**
- Might need to set up a proxy or SSH key if direct access is blocked.  
- If direct git clone doesn’t work, **the fallback** is to clone locally, zip the repo, and upload via `scp`.

---

### 3.3 Other Sources & Workarounds

- **Shared HPC Project Space**: If your lab or project has data stored centrally, you can `cp` or `ln -s` from that location.  
- **rsync** for large directories:  
  ```bash
  rsync -avz local_bigdata_dir <username>@stanage.shef.ac.uk:/mnt/parscratch/users/<username>/
  ```
  This is robust and resumable (only changes get synced).

---

## 4. Managing Data in HPC Directories

1. **Where to Put Data**  
   - **Home Directory**: `/users/my_auth/com6012/ScalableML/Data` – smaller quota, usually slower.  

2. **Check Quotas**  
   - HPC docs or `quota` command might show how much space you have.  
   - Large data sets can quickly exceed your home directory’s limit, so use `scratch`.

---

## 5. Practical Example: Multi-Source Data Processing on HPC

### 5.1 Overview of the Example

**Goal**: We’ll combine data from two places:
1. A local file `local_data.csv` (uploaded via SCP).
2. A GitHub repo `public_dataset.csv` (cloned from GitHub, or manually uploaded if HPC can’t directly clone).

We then run a Spark job that:
- Reads the two CSVs from HPC.  
- Performs some basic transformations.  
- Saves the combined result to HPC scratch.

---

### 5.2 Step-by-Step Demo

#### **Step A**: Upload Data From Multiple Sources

1. **Local CSV**  
   ```bash
   # On your local machine
   scp ./local_data.csv \
       <username>@stanage.shef.ac.uk:/users/my_auth/com6012/ScalableML/Data
   ```
2. **GitHub Data**  
   ```bash
   # If HPC can git clone:
   cd /users/my_auth/com6012/ScalableML/Data
   git clone https://github.com/YourAccount/YourDataRepo.git

   # Now your data is at multi_src_demo/YourDataRepo/public_dataset.csv
   ```
   Or, if that doesn’t work:
   - Clone it locally.
   - Upload the CSV(s) via scp or SFTP the same way as above.

3. **File Layout**  
   - `~/multi_src_demo/data/local_data.csv`  
   - `~/multi_src_demo/YourDataRepo/public_dataset.csv`  

---

## 6. Wrapping Up

1. **Plan Your Data Paths**: HPC is different from Kaggle/Colab. You usually upload data yourself.  
2. **Use HPC Scratch**: For big data, read/write from `/mnt/parscratch/users/<username>`.  
3. **GitHub Repos**: If direct cloning is blocked, clone locally and upload via scp.  
4. **Test Interactively, Then Batch**: Use an interactive `srun` session to **quickly** test your code. When stable, run `sbatch` for bigger, longer jobs.  
5. **Check Logs**: The `.txt` (SLURM output) is your best friend for debugging.

**Now you can seamlessly bring together multiple data sources—local, GitHub, or HPC-shared—under one Spark job.** This workflow mirrors common HPC usage patterns where web-based data is not directly accessible, but you can still get work done by carefully placing your data and code in the right HPC directories. Good luck and enjoy your HPC data-crunching journey!
