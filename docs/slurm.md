
# Managing a Raspberry Pi Cluster with Slurm and Open-MPI

Using Slurm to manage a Raspberry Pi cluster is an excellent way to leverage this job scheduler for high-performance computing (HPC) workloads, even in a small-scale environment. Slurm can help you manage resources, schedule tasks, and distribute workloads across multiple Raspberry Pi nodes.

## Key Benefits of Using Slurm on Raspberry Pi Clusters
- **Resource Management:** Slurm allocates resources (CPU, memory, etc.) across your Raspberry Pi nodes, ensuring efficient usage.
- **Job Scheduling:** It manages job queues, allowing users to submit jobs that will be executed when resources are available.
- **Scalability:** Slurm can scale from small Raspberry Pi clusters to large HPC clusters.
- **Customizable:** You can tailor Slurm to suit the specific needs of your workload, whether for educational purposes, hobby projects, or small-scale research.

## General Steps to Use Slurm on a Raspberry Pi Cluster

### 1. Cluster Setup
- **Hardware:** Raspberry Pi units connected via Ethernet.
- **Operating System:** Raspberry Pi OS (or any Linux-based distro like Ubuntu).
- **Networking:** Each Pi should have SSH access with static IPs.
- **Shared Storage:** Set up NFS (or other shared file systems) for job files and shared data across nodes.

### 2. Slurm Architecture
Slurm operates using three main components:
- **Slurm Controller (slurmctld):** The master node that manages job scheduling and distribution across compute nodes.
- **Compute Nodes (slurmd):** The worker Raspberry Pi nodes where the jobs are executed.
- **Database Daemon (slurmdbd) [Optional]:** Tracks job history and resource usage.

### 3. Installing Slurm
On all Raspberry Pi nodes (both master and worker):
```bash
sudo apt update
sudo apt install slurm-wlm munge nfs-common
```

Slurm uses **Munge** for authentication, so you need to set up Munge on all nodes:
- On the controller node (master):
```bash
sudo /usr/sbin/create-munge-key
sudo scp /etc/munge/munge.key worker:/etc/munge/
```
- On worker nodes, restart the Munge service:
```bash
sudo systemctl start munge
```

### 4. Configuring Slurm
- **Slurm Configuration:** On the master node, edit the `/etc/slurm/slurm.conf` file. Here’s a minimal configuration for a cluster with 4 Raspberry Pis:
```bash
ClusterName=raspi-cluster
SlurmctldHost=pi-master
NodeName=pi[1-4] CPUs=4 State=UNKNOWN
PartitionName=default Nodes=pi[1-4] Default=YES MaxTime=INFINITE State=UP
```
- **Database (Optional):** If you want to track job statistics, you can set up `slurmdbd` and connect it to a database like MySQL.

### 5. NFS Shared Storage Setup
- **NFS Server (on Master):**
```bash
sudo apt install nfs-kernel-server
sudo nano /etc/exports
```
Add a shared directory:
```bash
/home/shared *(rw,sync,no_subtree_check)
```
Start NFS:
```bash
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
```

- **NFS Client (on Workers):**
```bash
sudo apt install nfs-common
sudo mount pi-master:/home/shared /mnt/shared
```

### 6. Starting Slurm
- On the master node:
```bash
sudo systemctl start slurmctld
```
- On worker nodes:
```bash
sudo systemctl start slurmd
```

### 7. Submitting Jobs
Once Slurm is up and running, you can submit jobs via `sbatch`:
- Create a job script `job.sh`:
```bash
#!/bin/bash
#SBATCH --job-name=mytestjob
#SBATCH --output=output.txt
#SBATCH --ntasks=1
#SBATCH --time=00:05:00

echo "Hello from $(hostname)"
```
- Submit the job:
```bash
sbatch job.sh
```

### 8. Monitoring Jobs
- Use `squeue` to check job status:
```bash
squeue
```
- To view the node status:
```bash
sinfo
```

## Integrating Open-MPI for Parallel Jobs
To enhance your cluster for distributed workloads, you can integrate **Open-MPI** with Slurm, enabling you to run parallel jobs across multiple Raspberry Pi nodes.

### 1. Install Open-MPI on All Nodes
```bash
sudo apt install openmpi-bin libopenmpi-dev
```

### 2. Write an MPI Program
Here’s an example **C** program (`mpi_hello.c`):
```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    MPI_Init(NULL, NULL);

    int world_size;
    MPI_Comm_size(MPI_COMM_WORLD, &world_size);

    int world_rank;
    MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);

    printf("Hello from processor %d of %d\n", world_rank, world_size);

    MPI_Finalize();
    return 0;
}
```

### 3. Compile the Program
Use `mpicc` to compile the MPI code:
```bash
mpicc -o mpi_hello mpi_hello.c
```

### 4. Submit MPI Jobs with Slurm
Create a Slurm job script (`mpi_job.sh`) to run the MPI program:
```bash
#!/bin/bash
#SBATCH --job-name=mpi_hello
#SBATCH --output=mpi_output.txt
#SBATCH --ntasks=4

mpirun ./mpi_hello
```

Submit the job to the cluster:
```bash
sbatch mpi_job.sh
```

This setup allows you to harness the full power of your Raspberry Pi cluster for parallel computing tasks.

## Additional Considerations:
- **Hybrid MPI and OpenMP:** You can mix MPI (distributed parallelism) and OpenMP (shared memory parallelism) to optimize performance based on your workload.
- **Monitoring & Logging:** Slurm’s logging and monitoring capabilities can help track resource usage, optimize job scheduling, and identify bottlenecks.
