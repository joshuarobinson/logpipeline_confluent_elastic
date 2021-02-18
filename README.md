# cluster_install

Playbooks to install a log analytics cluster in kubernetes and start a simple load-generator based on [flog](https://github.com/mingrammer/flog).

The log analytics pipeline leverages 1) Confluent Tiered Storage and 2) Elastic Searchable Snapshots, both built upon FlashBlade S3 object store.
 * Flog produces synthetic http click data
 * Kafka configured to use Confluent Tiered Storage on S3 to simplify large-scale topics
 * Beats to pull data from Kafka to Elasticsearch
 * Elasticsearch with storage on FlashBlade NFS with an S3 snapshot repository for backups and cold tier storage

Further Reading:
* [Simplify Kafka at Scale with Confluent Tiered Storage](https://joshua-robinson.medium.com/simplify-kafka-at-scale-with-confluent-tiered-storage-ae8c1a2c9c80)

# Assumptions

* Kubernetes cluster installed and kubectl available locally.
* Ansible installed locally.
* PSO installed and configured correctly.
* (Optional) FlashArray/PSO or PortWorx configured.
* flashblade_creds.yaml (described later) file locally.

# How to use?

Use 'ansible-playbook' to run the following playbooks:

* system_install.yaml - Install one-time requirements.
* install_cluster.yaml - Create a log analytics pipeline.
* demo_playbook.yaml - Walk through increasing load, scaling, and showing failovers.

Adjust parameters in the variables section a the top of install-cluster.yaml.

After install_cluster.yaml completes, it will leave a file in the current directory "undo-cluster.sh" which is an uninstall script. Simply run the script *everything* from the pipeline should be removed.

# Inputs 

Requires the VIPs and login token in a file called [flashblade_creds.yaml](flashblade_creds.yaml)

To obtain the TOKEN, login via CLI and either create or list the token:
```pureadmin [create|list] --api-token --expose```
