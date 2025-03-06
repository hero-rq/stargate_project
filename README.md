
# **Stargate Setup & Running Jobs on HPC (for me and for general use)**  

## **1️⃣ Connecting to VPN & Authenticating**  
Connect to my VPN and do auth.  
```bash
ssh $USER@stanage.shef.ac.uk
```

## **2️⃣ Start an Interactive HPC Session**  
```bash
srun --pty bash -i
```

## **3️⃣ Load Required Modules (one by one)**  
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
top down attack and write any python code in the Code/ directory 

---

## **6️⃣ Submitting a Batch Job**  

Create a job script **`HPC/initial_top_down.sh`** (or anything I want .sh):  

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
spark-submit ./Code/topdown_attack.py #(the python file I wrote)
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
initial_topdown_state.txt  (and other files)
```

To view the batch job log:
```bash
cat initial_topdown_state.txt
```

---

### **✅ Done!**  
Now my job is executed, and I can check the results inside the **`Output/`** directory. 🎯

