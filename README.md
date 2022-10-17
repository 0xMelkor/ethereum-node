# eth-node-gcloud
A Kubernetes project to deploy a PoS Ethereum full node on Google GKE

# Mount machine local SSD into one partition

Docs https://cloud.google.com/compute/docs/disks/add-local-ssd

```bash
# Print volumes
lsblk

# Output is similar to this
andreasimeoni84@gke-crystal-mev-default-pool-c7428e3b-4xc6:~$ lsblk
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda       8:0    0  100G  0 disk 
├─sda1    8:1    0 99.9G  0 part /home/kubernetes/containerized_mounter/rootfs/var/lib/kubelet
│                                /home/kubernetes/containerized_mounter/rootfs/var/lib/kubelet
│                                /home/kubernetes/containerized_mounter
│                                /home/containerd
│                                /var/lib/kubelet
│                                /home/kubernetes/flexvolume
│                                /home/kubernetes/bin
│                                /
├─sda14   8:14   0    4M  0 part 
└─sda15   8:15   0  106M  0 part /boot/efi
sdb       8:16   0  375G  0 disk /mnt/disks/ssd0
sdc       8:32   0  375G  0 disk /mnt/disks/ssd1
sdd       8:48   0  375G  0 disk /mnt/disks/ssd2
sde       8:64   0  375G  0 disk /mnt/disks/ssd3
sdf       8:80   0  375G  0 disk /mnt/disks/ssd4
sdg       8:96   0  375G  0 disk /mnt/disks/ssd5
sdh       8:112  0  375G  0 disk /mnt/disks/ssd6
sdi       8:128  0  375G  0 disk /mnt/disks/ssd7

# Unmount drives to format them later
sudo umount /mnt/disks/ssd0
sudo umount /mnt/disks/ssd1
sudo umount /mnt/disks/ssd2
sudo umount /mnt/disks/ssd3
sudo umount /mnt/disks/ssd4
sudo umount /mnt/disks/ssd5
sudo umount /mnt/disks/ssd6
sudo umount /mnt/disks/ssd7

# Format drives to ext4
sudo mkfs.ext4 -F /dev/sdb
sudo mkfs.ext4 -F /dev/sdc
sudo mkfs.ext4 -F /dev/sdd
sudo mkfs.ext4 -F /dev/sde
sudo mkfs.ext4 -F /dev/sdf
sudo mkfs.ext4 -F /dev/sdg
sudo mkfs.ext4 -F /dev/sdh
sudo mkfs.ext4 -F /dev/sdi

# Merges 5 volumes into one
sudo apt update && sudo apt install mdadm --no-install-recommends
sudo mdadm --create /dev/md0 --level=0 --raid-devices=5 /dev/sdb /dev/sdc /dev/sdd /dev/sde /dev/sdf

# Format the new drive
sudo mkfs.ext4 -F /dev/md0

# Mount all

# Merged disk for geth
sudo mkdir -p /mnt/disks/ssd0
sudo mount /dev/md0 /mnt/disks/ssd0
sudo chmod a+w /mnt/disks/ssd0

# Reserved to lighthouse
sudo mkdir -p /mnt/disks/ssd1
sudo mount /dev/sdg /mnt/disks/ssd1
sudo chmod a+w /mnt/disks/ssd1

# For other applications and bots
sudo mkdir -p /mnt/disks/ssd2
sudo mount /dev/sdh /mnt/disks/ssd2
sudo chmod a+w /mnt/disks/ssd2

sudo mkdir -p /mnt/disks/ssd3
sudo mount /dev/sdi /mnt/disks/ssd3
sudo chmod a+w /mnt/disks/ssd3
```
