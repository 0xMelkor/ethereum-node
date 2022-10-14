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