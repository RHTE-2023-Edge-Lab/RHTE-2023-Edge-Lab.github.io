+++
title = "Kafka Broker"
weight = 5
chapter = true
pre = "<b>C. </b>"
+++

# Install the Kafka Broker

## Duo C - Create a Kafka user for Camel K integration

Create the secret containing camel user passord  

```sh
kind: Secret
apiVersion: v1
metadata:
  name: camel-user
  namespace: <your_namespace>
stringData:
  password: s3cr3t
type: Opaque
```

Create the Kafka user for Camel K  

```sh
---
apiVersion: kafka.strimzi.io/v1beta2
kind: KafkaUser
metadata:
  name: camel
  namespace: <your_namespace>
  labels:
    strimzi.io/cluster: warehouse
spec:
  authentication:
    password:
      valueFrom:
        secretKeyRef:
          name: camel-user
          key: password
    type: scram-sha-512
```

![KAFKA user](/images/warehouse-kafka-user.png)
