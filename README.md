# eth-node-gcloud
A Kubernetes project to deploy a PoS Ethereum full node on Google GKE

```bash
# Create cluster
gcloud container clusters create-auto crystal-mev \
--cluster-version="1.24.2-gke.1900" \
--region=us-central1 

# Create persistent disk for geth
gcloud compute disks create geth-disk \
--size=2000GB \
--type=pd-standard \
--zone=us-central1-a

# Create persistent disk for lighthouse
gcloud compute disks create lighthouse-disk \
--size=500GB \
--type=pd-standard \
--zone=us-central1-a
```



# Mount machine local SSD into one partition

```bash
# Print volumes
lsblk

# Merges 3 volumes into one, the 4th is left alone
sudo apt update && sudo apt install mdadm --no-install-recommends
sudo mdadm --create /dev/md0 --level=0 --raid-devices=3 /dev/sdb /dev/sdc /dev/sdd

# Format the new drive
sudo mkfs.ext4 -F /dev/md0
```