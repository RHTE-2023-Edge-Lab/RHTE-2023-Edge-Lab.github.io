+++
title = "Camel-K Integration"
weight = 6
chapter = true
pre = "<b>C. </b>"
+++



# Develop the Camel-K Integration

## Duo C - Create your Kafka broker in your warehouse namespace

```sh
apiVersion: kafka.strimzi.io/v1beta2
kind: Kafka
metadata:
  name: warehouse
  namespace: <your_namespace>
spec:
  kafka:
    config:
      offsets.topic.replication.factor: 3
      transaction.state.log.replication.factor: 3
      transaction.state.log.min.isr: 2
      default.replication.factor: 3
      min.insync.replicas: 2
      inter.broker.protocol.version: '3.2'
    storage:
      type: ephemeral
    listeners:
      - authentication:
          type: scram-sha-512
        name: plain
        port: 9092
        type: internal
        tls: false
      - authentication:
          type: scram-sha-512
        name: tls
        port: 9093
        type: route
        tls: true
    version: 3.2.3
    replicas: 3
  entityOperator:
    topicOperator: {}
    userOperator: {}
  zookeeper:
    storage:
      type: ephemeral
    replicas: 3
```

## Duo C - Create your Kafka IN and OUT topics in your broker

```sh
kind: KafkaTopic
apiVersion: kafka.strimzi.io/v1beta2
metadata:
  name: warehouse-in
  namespace: <your_namespace>
  labels:
    strimzi.io/cluster: warehouse
spec:
  config:
    retention.ms: 604800000
    segment.bytes: 1073741824
---
kind: KafkaTopic
apiVersion: kafka.strimzi.io/v1beta2
metadata:
  name: warehouse-out
  namespace: <your_namespace>
  labels:
    strimzi.io/cluster: warehouse
spec:
  config:
    retention.ms: 604800000
    segment.bytes: 1073741824
```

## Result

Kafka pods  

![KAFKA pods](/images/warehouse-kafka-pods.png)

Kafka instances  

![KAFKA instances](/images/warehouse-kafka-instances.png)