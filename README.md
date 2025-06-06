# Hybrid HPC and Big Data Cluster for Bioinformatics Analysis

## 📌 Project Overview
This project demonstrates a hybrid high-performance computing (HPC) and big data processing system designed to analyze bioinformatics data. We built a Mini-HPC cluster using MPI and a Docker-based Spark cluster to classify disease status based on gene and protein expression levels.

## 🚀 How to Run

### Task 1 – MPI Cluster
1. Set up 3 Ubuntu VMs using VirtualBox (master + 2 workers).
2. Configure static IPs and enable passwordless SSH.
3. Install `openmpi`, `python3`, `mpi4py`, `scikit-learn`, and `pandas`.
4. Copy the dataset (`cleaned_bio_dataset.csv`) to all nodes.
5. Run:
   ```bash
   mpirun -np 6 --hostfile hostfile python3 process_dataset_mpi.py
