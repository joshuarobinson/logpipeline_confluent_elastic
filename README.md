# Log Analytics Pipeline: Pure + Confluent + Elastic

A helm chart to install a log analytics cluster in kubernetes and start a simple load-generator based on [flog](https://github.com/mingrammer/flog).

The log analytics pipeline leverages 1) Confluent Tiered Storage and 2) Elastic Searchable Snapshots, both built upon FlashBlade S3 object store.
 * Flog produces synthetic http click data
 * Kafka configured to use Confluent Tiered Storage on S3 to simplify large-scale topics
 * Beats to pull data from Kafka to Elasticsearch
 * Elasticsearch with storage on FlashBlade NFS with an S3 snapshot repository for backups and cold tier storage

Further Reading:
* [Simplify Kafka at Scale with Confluent Tiered Storage](https://joshua-robinson.medium.com/simplify-kafka-at-scale-with-confluent-tiered-storage-ae8c1a2c9c80)
* [A Guide to Elasticsearch Snapshots](https://joshua-robinson.medium.com/a-guide-to-elasticsearch-snapshots-565017630638)

# Assumptions

* Kubernetes cluster installed and kubectl available locally.
* PSO installed and configured correctly.
* (Optional) FlashArray/PSO or PortWorx configured.

# Inputs 

Requires the VIPs and login token specified in the values.yaml.

To obtain the TOKEN, login via CLI and either create or list the token:
```pureadmin [create|list] --api-token --expose```

Specify StorageClasses in values.yaml, depending on your PSO (FlashArray or FlashBlade) or PortWorx availability.
