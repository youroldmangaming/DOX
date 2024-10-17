<img src="../dox.png" width="150" height="100" alt="YOMG Lab Documentation">
# DOX - A Computer Scientists NoteBook

<img src="../cluster.jpg"  alt="YOMG Lab Documentation">

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

<img src="../network_pi.png"  alt="YOMG Lab Documentation">


### Power Supply:
- **Power Cables and USB Power Supply** — Powering all the Raspberry Pi devices. Depending on your setup, a multi-port USB power supply will help minimize cable clutter.

Turning the cluster on and off via a single button can be conducted in one of two way, with wiring and a firmware level interaction via the onboard functionality. The second option is to write a program to detect the state on one device going into shutdown and cascading this across the custer. Having going down this second path with marginal positive outcomes the physical route has been determined to be the path of least resistence to get to the desirded outcome.

# RPI-Cluster Turn Off and Turn On

The cluster is to be turned on an off using a single push button.

The button has two wires +ve and -ve that get terminated in parallel to each of the 8 raspberry Pi's in the cluser to GPIO 5 and 6.

An update to the `sudo nano /boot/firmware/config.txt  ` add the below:

```
dtoverlay=gpio-shutdown
```
When the cluster is in an on state pressing and holding the button for 3 seconds will turn off all Rapberry Pis in the cluster.

This will put the Raspberry Pi's into a hybernated mode.

Once in this mode turning the cluster back on is again, just holding down the button.

<img src="../turnoff.png"  alt="YOMG Lab Documentation">





---
**DOX - A Computer Scientist's Notebook**  
_Y0MG 1990-2024_  
[GitHub Repository](https://github.com/youroldmangaming/DOX/tree/master) | [Documentation Site](https://dox.youroldmangaming.com)

---
