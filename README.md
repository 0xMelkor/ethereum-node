# Geth + Lighthouse kubernetes deployment on GKE

A Kubernetes project to deploy a PoS Ethereum full node on Google GKE using a Geth "execution layer" and a Lighthouse "consensus layer".

![Cluster architecture](./docs/images/architecture.png)

This architecture ca be deployed running the `cloudbuild.yaml` file from a Google cloudbuild console.

## InfluxDB
Geth can be instructed to send operational metrics to an InfluxDB instance. 

After deploying InfluxDB, connect to the pod and create a database and a user `INFLUX_DB_USERNAME` with password `INFLUX_DB_PASSWORD`. Keep these data in a safe place. 

```bash
kubectl get pods

# Obtain someting like this
NAME                                     READY   STATUS    RESTARTS      AGE
geth-deployment-7f74d4fb96-tlhhg         1/1     Running   0             42h
influxdb-deployment-0                    1/1     Running   0             42h
lighthouse-deployment-6fbc98c8b8-p89x6   1/1     Running   0             42h
nginx-deployment-7c776cbc96-f2lnw        1/1     Running   0             42h

# Obtain an InfluxDB shell
kubectl exec -it influxdb-deployment-0 bin/bash

# Run `influx`
root@influxdb-deployment-0: influx

# The InfluxDB cli will show up
Connected to http://localhost:8086 version 1.8.10
InfluxDB shell version: 1.8.10
> 
```
You can now issue the following commands to create a database for geth with username and password.
```
CREATE DATABASE "geth"
USE geth
CREATE USER "<INFLUX_DB_USERNAME>" WITH PASSWORD '<INFLUX_DB_PASSWORD>'
GRANT ALL ON "geth" TO "user"
```
Same credentials must be configured on the cloudbuild trigger as variables `_INFLUX_DB_USERNAME`, `_INFLUX_DB_PASSWORD`.

In order to collect geth metrics you need to enable corresponding lines in the the `./geth/deployments/geth-deployment.yaml`.

```yaml
# Enable the following lines to collect metrics on InfluxDB
 - "--metrics"
 - "--metrics.influxdb=true"
 - "--metrics.influxdb.endpoint=http://influxdb-service:8086"
 - "--metrics.influxdb.username=$(INFLUX_DB_USERNAME)"
 - "--metrics.influxdb.password=$(INFLUX_DB_PASSWORD)"
 - "--metrics.influxdb.database=geth"
```

## Expose the node 
You can expose your node deploying the nginx pod. All the required configuration can be found under the `./nginx` folder.
The current configuration exposes:
* Geth RPC Http
* Geth RPC WebSocket
* Influx DB

Nginx can be deployed running the `./nginx/cloudbuild.yaml` file from a Google cloudbuild console.

