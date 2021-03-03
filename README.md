# Log Analytics Pipeline: Pure + Confluent + Elastic

A helm chart to install a log analytics cluster in kubernetes and start a simple load-generator based on [flog](https://github.com/mingrammer/flog).

The log analytics pipeline leverages 1) Confluent Tiered Storage and 2) Elastic Searchable Snapshots, both built upon FlashBlade S3 object store.
 * [Flog](https://github.com/mingrammer/flog) is a fake log generator for apache weblog-like output
 * Kafka configured to use [Confluent Tiered Storage](https://docs.confluent.io/platform/current/kafka/tiered-storage.html) on S3 to simplify operations at scale
 * Filebeats to pull data from Kafka to Elasticsearch
 * Elasticsearch with storage on FlashBlade NFS with an S3 snapshot repository for backups and cold tier storage

As part of the setup process, this helm chart creates the necessary S3 accounts, users, keys, and buckets on the target FlashBlade using a separate program called s3manage. This program is a lightweight python wrapper to make using the REST API easier from a Kubernetes job.

Currently, the chart creates NodePort services to expose the Confluent Control Center (port :30921) and Kibana (port :30561) interfaces.

Further Reading:
* [Simplify Kafka at Scale with Confluent Tiered Storage](https://joshua-robinson.medium.com/simplify-kafka-at-scale-with-confluent-tiered-storage-ae8c1a2c9c80)
* [A Guide to Elasticsearch Snapshots](https://joshua-robinson.medium.com/a-guide-to-elasticsearch-snapshots-565017630638)

# Pre-requisites

* Kubernetes cluster installed
* [PSO installed](https://github.com/purestorage/helm-charts/tree/master/pure-csi) and configured correctly.
* (Optional) FlashArray/PSO or PortWorx configured.
* [Elastic Cloud for Kubernetes](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-installing-eck.html) installed.
* Elastic license enabled or [trial license installed](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-licensing.html#k8s-start-trial).

# Inputs 

Requires the VIPs and login token specified in the values.yaml.

To obtain the TOKEN, login via CLI and either create or list the token:

```pureadmin [create|list] --api-token --expose```

Specify StorageClasses in values.yaml, depending on your PSO (FlashArray or FlashBlade) or PortWorx availability.
