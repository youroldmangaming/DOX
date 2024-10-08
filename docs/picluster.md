<img src="dox.png" width="150" height="100" alt="YOMG Lab Documentation">
# DOX - A Computer Scientists NoteBook

<img src="comp.jpg"  alt="YOMG Lab Documentation">

For full source visit [github](https://github.com/youroldmangaming/DOX/).


<audio controls>
  <source src="ElevenLabs_2024-09-29T08_37_04_Grandpa Spuds Oxley_pvc_s67_sb97_t2.mp3" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>


# Building a Cluster using Raspberry Pi's

# Building a Miniature Community-Style Cluster with Raspberry Pi 5 and Open Source HPC Software

This guide documents the process of building a miniature, open-source cluster using Raspberry Pi computers and high-performance computing (HPC)-grade software. The focus is on creating a low-cost home lab environment that is scalable and adaptable to various setups.

When I first started this project, I found that detailed guides were surprisingly sparse, especially for setting up modern Pi clusters with HPC tools. As I document my progress, I hope to provide a series of guides for others embarking on similar projects. This first guide covers the basics: **setting up the hardware** and **getting a cluster scheduler running**.

---

## Step 0: Get The Hardware

Here’s the parts list for my cluster:

### Compute Nodes:
- **6x Raspberry Pi 5 (8GB)** — These will act as the primary compute nodes, handling most of the heavy lifting during task execution.

### Master Node:
- **1x Raspberry Pi 4 (8GB)** — This will be used as the master or login node, handling job submission, scheduling, and management.

### Additional Infrastructure:
- **1x Mac Mini (2013)** — This will serve as an auxiliary node, possibly for additional services or as a backup node. It can also assist with tasks like file serving, monitoring, or handling external access.

### Networking:
- **1x 8-port Gigabit Network Switch** — This switch will connect all devices, allowing fast communication between nodes.

### Power Supply:
- **Power Cables and USB Power Supply** — Powering all the Raspberry Pi devices. Depending on your setup, a multi-port USB power supply will help minimize cable clutter.

---

## Step 1: Storage Setup

In a cluster environment, **shared storage** is critical. This allows all nodes to work on the same set of files simultaneously. There are two main options for storage in this setup:

### 1. USB Drive as Shared Storage:
A simple, low-cost option is to plug a 64GB USB drive into one of the Pis (typically the master node) and set it up as a shared drive.

1. Format the USB drive to a suitable filesystem (e.g., ext4).
2. Mount the USB drive on the master node, then set it up as a Network File System (NFS) share.

**NFS Setup on Master Node:**

1. Install NFS server on the master node:
   ```bash
   sudo apt-get install nfs-kernel-server


---
**DOX - A Computer Scientist's Notebook**  
_Y0MG 1990-2024_  
[GitHub Repository](https://github.com/youroldmangaming/DOX/tree/master) | [Documentation Site](https://dox.youroldmangaming.com)

---
