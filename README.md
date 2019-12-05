This is an IoT project conducted by students of the UAS JOANNEUM as part of the bachelor's programme of [Information Management](https://www.fh-joanneum.at/informationsmanagement/bachelor/). The goal was to build two IoT pipelines. One using Kafka, Kafka Connect and Kafka Streams for ingestion and stream processing. The other uses MQTT, RabbitMQ as broker and Apache Flink as stream processing engine. Both pipelines persist data in an InfluxDB (Time-Series DB) and visualize data with Grafana.

After both pipelines have been deployed, they are load tested with Jmeter.

# Responsible Persons

* @AnotherCodeArtist is the Mentor and guiding person of this project.

* @GregorFernbach is responsible for MQTT Data ingestion and Streamprocessing with Apache Flink.

* @gzei is responsible for the infrastructure (Kubernetes, NFS, Hardware and VMs).

* @vollmerm17 is responsible for Kafka and Streamprocessing with Kafka Streams.

* @lachchri16 is responsible for visualisation.

* @cynze is responsible for the databases and the connections between Kafka and those databases.

All members are responsible for the loadtesting.

# Architecture

# Requirements

* Debian 10.1 servers
* external NFS server for centralized storage
* SSDs are highly recommended
** Apache Flink **
* Maven
* Java
* IntelliJ as IDE recommended

# Structure

## K8s

In this folder all necessary files for recreating the Kubernetes cluster are stored. This includes:

* The Ansible Playbook for installing the prerequisites
* An example cluster.yml used for our implementation. This cluster utilizes three nodes, where the first one is a controlplane and etcd node.
the two remaining nodes are etcd nodes as well as worker nodes.

## Kafka

## Databases

## ApacheFlink

Is the Stream Processing Engine for the MQTT pipeline.

For the deployment of Apache Flink  the official 'yml' files from the homepage (https://ci.apache.org/projects/flink/flink-docs-stable/ops/deployment/kubernetes.html) have been used. Moreover they are accessible under ApacheFlink\Deployment.

Under ApacheFlink\StreamingJobs the source code of the Streaming Jobs can be found. There are:

- DetectionJob-rmq: Represents the whole RabbitMQ Streaming Job with Persisting raw Sensor Data, processed Sensor Data and Area Output with Telegram.
- DetectionJob-rmq-loadtest: Represents the partial RabbitMQ Streaming Job which only persists raw Sensor Data and processed Sensor Data for the load test.
- DetectionJob-kafka: Represents the whole Kafka Streaming Job with Persisting raw Sensor Data, processed Sensor Data and Area Output with Telegram.
- DetectionJob-kafka-loadtest: Represents the partial Kafka Streaming Job which only persists raw Sensor Data and processed Sensor Data for the load test.

In order to create '.jar' files out of the source code, you need maven installed and have to run 'mvn clean install'.
Then you can upload the jar to flink and submit with your parallelism properties.

For configuring the RabbitMQ connector see the [Official Docs]: https://ci.apache.org/projects/flink/flink-docs-stable/dev/connectors/rabbitmq.html and the [Source Code]: https://github.com/apache/flink/tree/master/flink-connectors/flink-connector-rabbitmq/src/main/java/org/apache/flink/streaming/connectors/rabbitmq.

For configuring the Apache Kafka connector see the [Official Docs]: https://ci.apache.org/projects/flink/flink-docs-stable/dev/connectors/kafka.html, and also the [Training from Veverica]: https://training.ververica.com/exercises/toFromKafka.html.

For configuring the InfluxDB connector see the [Apache Bahir Docs]: https://bahir.apache.org/docs/flink/current/flink-streaming-influxdb/ from which this connector comes from.

For configuring the telegram API (using Botfather) see the [Official Docs]: https://core.telegram.org/

## JMeter

In this folder a fork of the https://github.com/BrightTag/kafkameter is stored. The changes include:

* Support for generating message Keys.
* Support for setting Sensor IDs for the message.
* Inside the Generators folder, the loadgenerator implementation can be found. For build instructions please consult the corresponding .bat file.

This folder also includes the testplans (JMeter version 5.1.1) used for testing Kafka and MQTT.

# Setup

## Kubernetes

1. Set up one or more VMs with Debian 10.1
2. Set up Ansible for those VMs
3. Apply Ansible Playbook provided in the k8s folder of this repository
4. Install rke 0.3.2 or newer on any node
5. Set up ssh key authentication from this node to all nodes in the cluster
6. Modify cluster.yml or generate new one (rke config)
7. Apply this config with rke up
8. Connect to the cluster with the generated kubectl config
9. Install Helm and Tiller
10. Use Helm to install nfs-client storage provider (https://github.com/kubernetes-incubator/external-storage/tree/master/nfs-client) and set as default storage class
11. Install MetalLB LoadBalancer (https://metallb.universe.tf/installation/)
12. Optional: Install Kubernetes Dashboard

## Databases

## MQTT

## Kafka
