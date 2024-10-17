<img src="../dox.png" width="150" height="100" alt="YOMG Lab Documentation">
# DOX - A Computer Scientists NoteBook

<img src="../GlusterFS.svg"  alt="YOMG Lab Documentation">

For full source visit [github](https://github.com/youroldmangaming/DOX.git).


# Managing a Raspberry Pi Distibuted File Share using Gluster


### 1. Check Cluster Availability in Preparation for  Installation
Checks the availability of nodes via slurm for administration(`is_node_alive.sh`):
```c
#!/bin/bash

# Function to check if a node is ready
check_node_ready() {
    local node=$1
    state=$(sinfo -n $node -o "%T" -h)
    if [[ $state == "idle" ]]; then
        echo "Ready"
    else
        echo "Not Ready"
    fi
}

# Function to make a node ready
make_node_ready() {
    local node=$1
    echo "Making node $node ready..."
    scontrol update nodename=$node state=idle
}

# Function to restart a node
restart_node() {
    local node=$1
    echo "Restarting node $node..."
    scontrol reboot $node
}

# Get all nodes
nodes=$(sinfo -h -o "%n")

# Main loop
while true; do
    not_ready_nodes=()
    all_ready=true

    echo "Checking node readiness..."
    for node in $nodes; do
        status=$(check_node_ready $node)
        echo "Node $node: $status"
        if [[ $status != "Ready" ]]; then
            all_ready=false
            not_ready_nodes+=($node)
        fi
    done

    if $all_ready; then
        echo "All nodes are ready for processing."
        break
    else
        echo "The following nodes are not ready:"
        printf '%s\n' "${not_ready_nodes[@]}"

        read -p "Do you want to make these nodes ready? (y/n): " make_ready
        if [[ $make_ready == "y" ]]; then
            for node in "${not_ready_nodes[@]}"; do
                make_node_ready $node
            done
        else
            echo "Exiting without making nodes ready."
            exit 1
        fi
    fi
done

# Options after all nodes are ready
while true; do
    echo "All nodes are ready. Choose an option:"
    echo "1. Quit"
    echo "2. Restart ready nodes"
    read -p "Enter your choice (1 or 2): " choice

    case $choice in
        1)
            echo "Exiting script."
            exit 0
            ;;
        2)
            echo "Restarting all ready nodes..."
            for node in $nodes; do
                if [[ $(check_node_ready $node) == "Ready" ]]; then
                    restart_node $node
                fi
            done
            echo "All ready nodes have been restarted."
            exit 0
            ;;
        *)
            echo "Invalid choice. Please enter 1 or 2."
            ;;
    esac
done


```

### 2. Remove all old GlusterFS shares (`slurm_remove_cluster_worker.sh`):

```bash
srun --nodelist=rpi51,rpi52,rpi53,rpi54,rpi41 --exclusive -N 5 bash /clusterfs/remove_gluster/remove_gluster_worker.sh
```
This file will remove current GlusterFS volumes(`remove_gluster_worker.sh`):
```c
#!/bin/bash

# Configuration variables
GLUSTERFS_DIR="/glusterfs"

echo "Starting GlusterFS cleanup on worker node"

# Stop GlusterFS services
echo "Stopping GlusterFS services"
systemctl stop glusterd
systemctl disable glusterd

# Remove GlusterFS packages
echo "Removing GlusterFS packages"
apt remove --purge glusterfs-server -y
apt remove --purge glusterfs-client -y
apt remove --purge glusterfs-common -y

apt autoremove -y

# Remove GlusterFS directories
echo "Removing GlusterFS directories"
rm -rf $GLUSTERFS_DIR /var/lib/glusterd /var/log/glusterfs /etc/glusterfs

# Remove GlusterFS mount from /etc/fstab
echo "Removing GlusterFS mount from /etc/fstab"
sed -i '/glusterfs/d' /etc/fstab

# Unmount any remaining GlusterFS mounts
#echo "Unmounting any remaining GlusterFS mounts"
#sudo umount -f /mnt || true

echo "GlusterFS cleanup on worker node completed successfully!"


systemctl stop glusterd.service
systemctl disable glusterd.service
rm /etc/systemd/system/glusterd.service
rm /etc/systemd/system/glusterd.service # and symlinks that might be related
rm /usr/lib/systemd/system/glusterd.service 
rm /usr/lib/systemd/system/glusterd.service # and symlinks that might be related
systemctl daemon-reload
systemctl reset-failed

```

This file will remove current GlusterFS volume from the manager node rpi41(`gluster_manager_clean.sh  `):

```c


#!/bin/bash

# Configuration variables
MANAGER_NODE="rpi41"
WORKER_NODES="rpi51,rpi52,rpi53,rpi54"
GLUSTERFS_DIR="/glusterfs"
VOLUME_NAME="staging-gfs"

echo "Starting GlusterFS cleanup on manager node: $MANAGER_NODE"

# Stop and delete the volume
echo "Stopping and deleting GlusterFS volume: $VOLUME_NAME"
gluster volume stop $VOLUME_NAME --mode=script || true
gluster volume delete $VOLUME_NAME --mode=script || true

# Detach peers (worker nodes)
IFS=',' read -ra WORKER_NODE_ARRAY <<< "$WORKER_NODES"
for node in "${WORKER_NODE_ARRAY[@]}"; do
    echo "Detaching peer: $node"
    gluster peer detach $node --mode=script || true
done

# Stop GlusterFS services
echo "Stopping GlusterFS services"
systemctl stop glusterd
systemctl disable glusterd

# Remove GlusterFS packages
echo "Removing GlusterFS packages"
apt purge -y glusterfs-server glusterfs-client glusterfs-common
apt autoremove -y

# Remove GlusterFS directories
echo "Removing GlusterFS directories"
rm -rf $GLUSTERFS_DIR /var/lib/glusterd /var/log/glusterfs /etc/glusterfs

# Remove GlusterFS mount from /etc/fstab
echo "Removing GlusterFS mount from /etc/fstab"
sed -i '/glusterfs/d' /etc/fstab

# Unmount any remaining GlusterFS mounts
echo "Unmounting any remaining GlusterFS mounts"
umount -f /mnt || true

echo "GlusterFS cleanup on manager node completed successfully!"

apt remove glusterfs-server glusterfs-client glusterfs-common -y
```



This script will install GlusterFS on all nodes with the cluster (manager and worker)(`install_gluster.sh  `):

```c


#!/bin/bash

# Define node names
MANAGER_NODE="rpi41"
WORKER_NODES=("rpi41" "rpi51" "rpi52" "rpi53" "rpi54")

# Function to run commands on a specific node
run_on_node() {
    echo $node
    local node=$1
    shift
    srun --nodes=1 --nodelist=$node "$@"
}

# Step 1: Install GlusterFS on all nodes
for node in $MANAGER_NODE "${WORKER_NODES[@]}"; do
    
    run_on_node $node sudo apt update
    run_on_node $node sudo apt install glusterfs-server -y
    run_on_node $node sudo systemctl status --now glusterd.service
    run_on_node $node sudo systemctl enable --now glusterd.service
    run_on_node $node sudo systemctl start --now glusterd.service
    run_on_node $node sudo systemctl status --now glusterd.service
    run_on_node $node sudo mkdir -p /glusterfs
done

```

This script will setup the workers nodes to the manager node(`setup_peering.sh  `), this script must be run on the manager node (rpi41):

```c

!/bin/bash

gluster peer probe rpi51
gluster peer probe rpi52
gluster peer probe rpi53
gluster peer probe rpi54
```

This script will setup the GlusterFS volumns and share them. To note this is not a setup similar to SAMBA or NFS where a central server is required to be up so that that files can be accessed. Gluster as setup in this instance, is fully redundant and distributed(`setup_gluster_distributed_share.sh `). This needs to be run on each node including the manager node:

```c
cd/
mkdir -p /glusterfs /mnt/glusterfs

echo "$hostname:/glusterfs /mnt/glusterfs glusterfs defaults,_netdev,backupvolfile-server= 0 0" >> /etc/fstab


chown -R root:docker /mnt
systemctl daemon-reload
mount -a
mount.glusterfs $hostname:/glusterfs /mnt
```

To be run on the manager node to build and startup the volume
```bash

gluster volume create glusterfs replica 5 rpi41:/glusterfs rpi51:/glusterfs rpi52:/glusterfs rpi53:/glusterfs rpi54:/glusterfs force
sudo gluster volume start glusterfs
```



<div style="font-size: 50%;">
  <pre><code>
  ┌────────────────────────────────────────────────────────────────────────┐   
  │                                                                        │   
  │ @@@@@@@@@@@@@@@@@@@@          %@@@@@@@@%          %@@            %@@   │   
  │ @@@@@@@@@@@@@@@@@@@@       %@@@@@@@@@@@@@@%     %@@@@@@        *@@@@@@ │   
  │ @@@@@@@@@@@@@@@@@@@@      @@@@@@@@@@@@@@@@@@     @@@@@@@%     @@@@@@%  │   
  │ @@@@@          %@@@@     %@@@@@%       @@@@@@      @@@@@@@% @@@@@@%#   │   
  │ @@@@@          %@@@@    %@@@@@          @@@@@@       @@@@@@@@@@@@#     │   
  │ @@@@@          %@@@@    @@@@@@          #@@@@@         @@@@@@@@%       │   
  │ @@@@@          %@@@@    @@@@@@          %@@@@@        %@@@@@@@@@       │   
  │ @@@@@          %@@@@    *@@@@@*         @@@@@%      #@@@@@@@@@@@@@     │   
  │ @@@@@@@@@@@@@@@@@@@@     @@@@@@@%     @@@@@@@     #@@@@@@@  @@@@@@@@   │   
  │ @@@@@@@@@@@@@@@@@@@@      *@@@@@@@@@@@@@@@@%    *@@@@@@@      @@@@@@@% │   
  │ @@@@@@@@@@@@@@@@@@@@        %@@@@@@@@@@@@@       @@@@@%         @@@@@% │   
  │                                %@@@@@@%            @%             @%   │   
  │                                                                        │   
  └────────────────────────────────────────────────────────────────────────┘
                                           A Computer Scientist's Notebook
                                                            Y0MG 1990-2024
GitHub Repository https://github.com/youroldmangaming/DOX
Documentation Site https://dox.youroldmangaming.com
  </code></pre>
</div>
