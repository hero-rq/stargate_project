
# **Stargate Setup & Running Jobs on HPC**  

## **1️⃣ Connecting to VPN & Authenticating**  
Ensure you are connected to your VPN and authenticated before proceeding.  

## **2️⃣ Start an Interactive HPC Session**  
```bash
srun --pty bash -i
```

## **3️⃣ Load Required Modules**  
```bash
module load Java/17.0.4
module load Anaconda3/2024.02-1
source activate myspark
export LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8
pip install matplotlib pandas
```

## **4️⃣ Navigate to the Project Directory**  
```bash
cd com6012/ScalableML/
```

---

## **5️⃣ Writing a Python Script in `Code/` Directory**  
Place your Python script inside the `Code/` directory to be executed within the HPC environment.

---

## **6️⃣ Submitting a Batch Job**  

Create a job script **`HPC/initial_top_down.sh`**:  

```bash
#!/bin/bash
#SBATCH --job-name=panda_kumo   # Replace with a meaningful job name
#SBATCH --time=00:30:00         # Adjust the runtime if needed
#SBATCH --nodes=1               # Number of nodes requested
#SBATCH --mem=4G                # Memory allocation (4GB)
#SBATCH --output=./Output/initial_topdown_state.txt  # Log output location

module load Java/17.0.4
module load Anaconda3/2024.02-1

source activate myspark
```

### **Submit the Job**  
```bash
sbatch HPC/initial_top_down.sh
```
✔️ This will return a **job ID**, e.g.:
```
Submitted batch job 5542113
```

---

## **7️⃣ Checking the Job Output**  
```bash
cd Output
ls
```
Example output:
```
COM6012_Lab1_SAMPLE.txt  COM6012_Lab1.txt  COM6012_Lab2.txt  
initial_topdown_state.txt  output_27_0.png
```

To view the batch job log:
```bash
cat initial_topdown_state.txt
```

---

### **✅ Done!**  
Now your job is executed, and you can check the results inside the **`Output/`** directory. 🎯

Let me know if you want to tweak anything! 😃🔥
