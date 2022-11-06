# Geth + Lighthouse kubernetes deployment on GKE
A Kubernetes project to deploy a PoS Ethereum full node on Google GKE using Geth "execution layer" and Lighthouse "consensus layer".


![Cluster architecture](./docs/images/architecture.png)

# InfluxDB 
```
CREATE DATABASE "geth"
USE geth
CREATE USER "user" WITH PASSWORD 'password'
GRANT ALL ON "geth" TO "user"
```
