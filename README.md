# Hybrid HPC and Big Data Cluster for Bioinformatics Disease Classification

## Project Overview

This project implements a hybrid computing infrastructure combining traditional High-Performance Computing (HPC) using MPI and a Big Data framework using Dockerized Apache Spark. It is designed to analyze bioinformatics datasets containing gene and protein expression levels for disease classification. The goal is to compare the effectiveness of MPI-based parallel processing and PySpark-based distributed machine learning workflows in a virtualized cluster environment.

## How to Run

### Task 1 – MPI Cluster (Mini-HPC)

1. **Set up VMs**:

   * Create 3 Ubuntu Server 20.04 VMs using VirtualBox (1 master, 2 workers).
   * Configure NAT or Bridged networking and assign static IPs.

2. **Install Dependencies**:

   ```bash
   sudo apt update
   sudo apt install -y python3 python3-pip openmpi-bin libopenmpi-dev
   pip3 install mpi4py scikit-learn pandas
   ```

3. **Enable Passwordless SSH**:

   * Generate SSH keys with `ssh-keygen`
   * Share keys using `ssh-copy-id` to all nodes

4. **Transfer Files**:

   * Copy `process_dataset_mpi.py` and `cleaned_bio_dataset.csv` to all nodes
   * Prepare a `hostfile` with all node IPs and slots

5. **Run MPI Script**:

   ```bash
   mpirun -np 6 --hostfile hostfile python3 process_dataset_mpi.py
   ```

### Task 2 – Spark Cluster (Hybrid Big Data)

1. **Install Docker** on all nodes:

   ```bash
   sudo apt install -y docker.io
   sudo systemctl start docker && sudo systemctl enable docker
   sudo usermod -aG docker $USER
   ```

2. **Initialize Docker Swarm** (on master):

   ```bash
   docker swarm init --advertise-addr <master_ip>
   ```

   * Join workers with `docker swarm join --token ...`

3. **Deploy Spark**:

   ```bash
   docker stack deploy -c spark-swarm.yml sparkcluster
   docker service ls
   ```

4. **Submit Spark Job**:

   ```bash
   docker cp bio_classifier.py <container_id>:/opt/bitnami/spark/
   docker cp cleaned_bio_dataset.csv <container_id>:/opt/bitnami/spark/
   docker exec -it <container_id> bash
   spark-submit --master spark://spark-master:7077 bio_classifier.py
   ```

## 🛠️ Technologies Used

* Python 3.8
* OpenMPI & mpi4py
* scikit-learn & pandas
* Apache Spark (via Bitnami Docker image)
* Docker & Docker Swarm
* VirtualBox + Ubuntu 20.04

## 📈 Results

* **MPI Task**: Parallel logistic regression achieved consistent results across all nodes; expression comparison saved as CSV.
* **Spark Task**: Logistic regression in PySpark returned accuracy metrics; evaluated on the same dataset in a scalable manner.
* Output files: `expression_comparison_rankX.csv`, `ml_results_rankX.txt`, `result.txt`

## 📸 Screenshots

![SSH Setup](screenshots/ssh_success.png)
![MPI Output](screenshots/mpi_output.png)
![Spark UI](screenshots/spark_ui.png)

## 📂 Project Structure

```
project_hpc_hybrid_cluster/
├── bio_classifier.py
├── cleaned_bio_dataset.csv
├── process_dataset_mpi.py
├── hostfile
├── spark-swarm.yml
├── Final_Report.pdf
├── screenshots/
│   ├── ssh_success.png
│   ├── mpi_output.png
│   └── spark_ui.png
├── expression_comparison_rank0.csv
├── expression_comparison_rank1.csv
├── ml_results_rank0.txt
├── ml_results_rank1.txt
├── result.txt
└── README.md
```

## 👥 Authors

* **Farah Ibrahim** – 221001140
* **Malak Atef** – 221000906
* **Zeina Ahmed** – 221000417

## 📚 License

This project is developed for educational purposes as part of the CBIO312: High Performance Computing course under Dr. Mohamed El-Sayeh, Spring 2025.
