# eth-node-gcloud
A Kubernetes project to deploy a PoS Ethereum full node on Google GKE

# Create "standard" cluster

```bash
# Create cluster
gcloud beta container  \
--project "defi-360312" \
clusters create "crystal-mev" \
--zone "us-central1-c" \
--no-enable-basic-auth \
--cluster-version "1.24.5-gke.600" \
--release-channel "None" \
--machine-type "c2-standard-8" \
--image-type "UBUNTU_CONTAINERD" \
--disk-type "pd-balanced" \
--disk-size "100" \
--local-ssd-count "4" \
--metadata disable-legacy-endpoints=true \
--scopes "https://www.googleapis.com/auth/devstorage.read_only","https://www.googleapis.com/auth/logging.write","https://www.googleapis.com/auth/monitoring","https://www.googleapis.com/auth/servicecontrol","https://www.googleapis.com/auth/service.management.readonly","https://www.googleapis.com/auth/trace.append" \
--max-pods-per-node "110" \
--spot --num-nodes "1" \
--logging=SYSTEM,WORKLOAD \
--monitoring=SYSTEM \
--enable-ip-alias \
--network "projects/defi-360312/global/networks/default" \
--subnetwork "projects/defi-360312/regions/us-central1/subnetworks/default" \
--no-enable-intra-node-visibility \
--default-max-pods-per-node "110" \
--no-enable-master-authorized-networks \
--addons HorizontalPodAutoscaling,HttpLoadBalancing,GcePersistentDiskCsiDriver \
--no-enable-autoupgrade \
--no-enable-autorepair \
--max-surge-upgrade 1 \
--max-unavailable-upgrade 0 \
--enable-shielded-nodes \
--node-locations "us-central1-c"
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
