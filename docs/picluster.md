<img src="dox.png" width="150" height="100" alt="YOMG Lab Documentation">
# DOX - A Computer Scientists NoteBook

<img src="comp.jpg"  alt="YOMG Lab Documentation">

For full source visit [github](https://github.com/youroldmangaming/DOX/).


<audio controls>
  <source src="../pi0.mp3" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>


# Building a Cluster using Raspberry Pi's

# Building a Miniature Home Lab Cluster with Raspberry Pi 5 and Open Source HPC Software

This guide documents the process of building a miniature, open-source cluster using Raspberry Pi computers and high-performance computing (HPC)-grade software. The focus is on creating a low-cost home lab environment that is scalable and adaptable to various setups.

When I first started this project, I found that detailed guides were surprisingly sparse, especially for setting up modern Pi clusters with HPC tools. As I document my progress, I hope to provide a series of guides for others embarking on similar projects. This first guide covers the basics: **setting up the hardware** and **getting a cluster scheduler running**.

---

## Step 0: Get The Hardware

Here’s the parts list for my cluster:

### Compute Nodes:
- **6x Raspberry Pi 5 (8GB)** — These will act as the primary compute nodes, handling most of the heavy lifting during task execution. They are relatively cheap and small and powerful, so as you need to scale, from a home labbers perspective, keeping these parameters under control is crucial.

### Master Node:
- **1x Mac Mini (2013)** — This will be used as the master or login node, handling job submission, scheduling, and management. Initially I thought that this would be too out of date. But after I installed linux(Proxmox on bear metal), it performed very well.

### Additional Infrastructure:
- **1x Raspberry Pi 4 (8GB)** —
- This will serve as an auxiliary node, for additional services or as a backup node. It can also assist with tasks like file serving, monitoring, or handling external access.


### Networking:
- **4x 4-port Wifi Network Switches** — These switches will connect all devices, allowing fast communication between nodes. They are meshed together over wifi and allocate static IPs. This will allow for flexibility in transporting the solution around the lab but also allow you to isolate the solution, as required, from the home network as you can saturate your daughters online game streaming business, so save yourself a headache.  

### Power Supply:
- **Power Cables and USB Power Supply** — Powering all the Raspberry Pi devices. Depending on your setup, a multi-port USB power supply will help minimize cable clutter.


### Software and Storage Setup

- **In a cluster environment, **shared storage** is critical. This allows all nodes to work on the same set of files simultaneously. 

In a cluster having a central store that all nodes can access is a requirement.

With the release of the Raspberry Pi 5, small and fast M.2 physical storage is now available. So each node will have this to run and load the OS and workloads. There will be local backup onto SD Cards, but also recovery backups into the cloud.

Shares across the cluster will be two fold, using traditional NFS as well as Gluster, more about that later.

Slurm will be used to automate and orchestrate jobs across the cluster and OpenMPI will be used to manage the inter CPU communication between the Raspberry Pi's.

I will be building a Python and R demo to show case this working.

---
**DOX - A Computer Scientist's Notebook**  
_Y0MG 1990-2024_  
[GitHub Repository](https://github.com/youroldmangaming/DOX/tree/master) | [Documentation Site](https://dox.youroldmangaming.com)

---
