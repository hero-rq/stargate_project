
```markdown
# **HPC Data Sourcing & Processing: Practical Guide**

---

## 1. Quick HPC Context

(security property) 
**you can still fetch data** through:

- **Kaggle datasets** via the `kaggle` CLI.
- **KaggleHub** for versioned, Python-native access.
- **FTP** downloads for legacy datasets (e.g., NASA logs).
- **GitHub clone**, if allowed on your cluster.
- ...

---

## 2. Preparing the HPC Environment

1. **Log into HPC**:
   ```bash
   ssh <username>@stanage.shef.ac.uk
```

2. **Start an interactive session**:
    
    ```bash
    srun --pty bash -i
    ```
    
3. **Load required modules**:
    
    ```bash
    module load Java/17.0.4
    module load Anaconda3/2024.02-1
    source activate myspark
    export LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8
    ```
    

---

## 3. Getting Data Onto HPC

### 3.1 Using Kaggle CLI

You can directly download Kaggle datasets inside your current directory :

```bash
cd /users/<your_username>/com6012/ScalableML/Data
kaggle datasets download -d brandao/diabetes --unzip
```

This will fetch and extract the dataset right there — no need for extra flags or root access.

---

### 3.2 Using FTP for Legacy Datasets (e.g. NASA Logs)

Some golden datasets still live on **public FTP servers**. You can grab them like this:

```bash
cd /users/<your_username>/com6012/ScalableML/Data
wget ftp://ita.ee.lbl.gov/traces/NASA_access_log_Jul95.gz
gunzip NASA_access_log_Jul95.gz
```

Simple, direct, works beautifully.

---

## 4. Managing Data in HPC Directories

- ✅ **Use your home Data directory**: `/users/<your_username>/com6012/ScalableML/Data`
    
- For huge files, consider `/mnt/parscratch/users/<username>/`
    

Always organize your data by project — for example:

```
~/com6012/ScalableML/
├── Code/
├── Data/
│   ├── diabetes.csv
│   ├── NASA_access_log_Jul95
```

---

## 5. Example: Using Multi-Source Datasets on HPC

Once you’ve downloaded datasets using Kaggle CLI, FTP, your directory might look like:

```
Data/
├── diabetes.csv
├── NASA_access_log_Jul95
```

Now you can write python code to read and process them.

---

## 6. Wrapping Up

- 💾 Use `kaggle datasets download`, or `wget` (FTP) to get data.
    
- 🧠 Work inside your project directory — avoid system folders.
    
- ⚡ HPCs are fast and powerful — once your data is there, you're ready to scale.
    

Enjoy crunching that data, Spark-style! 🚀
