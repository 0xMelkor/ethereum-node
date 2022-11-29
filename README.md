# Geth + Lighthouse kubernetes deployment on GKE

A Kubernetes project to deploy a PoS Ethereum full node on Google GKE using Geth "execution layer" and Lighthouse "consensus layer".

![Cluster architecture](./docs/images/architecture.png)

# InfluxDB

Geth can be instructed to send operational metrics to an InfluxDB instance. Please enable corresponding lines in the the `./geth/deployments/geth-deployment.yaml`:

```yaml
# Enable the following lines to collect metrics on InfluxDB
 - "--metrics"
 - "--metrics.influxdb=true"
 - "--metrics.influxdb.endpoint=http://influxdb-service:8086"
 - "--metrics.influxdb.username=$(INFLUX_DB_USERNAME)"
 - "--metrics.influxdb.password=$(INFLUX_DB_PASSWORD)"
 - "--metrics.influxdb.database=geth"
```

After deploying InfluxDB, connect to the pod and create a database and a user `INFLUX_DB_USERNAME` with password `INFLUX_DB_PASSWORD`. Keep these data in a safe place.

CREATE DATABASE "geth"
USE geth
CREATE USER "<INFLUX_DB_USERNAME>" WITH PASSWORD '<INFLUX_DB_PASSWORD>'
GRANT ALL ON "geth" TO "user"

```

```
