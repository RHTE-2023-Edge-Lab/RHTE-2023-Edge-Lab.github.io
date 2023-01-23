---
title: "Create the incoming and outcoming Kafka Topics (duo C)"
draft: false
weight: 3
---

Create the incoming and outcoming Kafka Topics by deploying these CRDs. One KafkaTopic is used for incoming parcels scanned by one of your ESP826. The other KafkaTopic is used for outcoming parcels scanned by the other ESP826.

First create the KafkaTopic for incoming traffic.  
To do so, connect to the OpenShift console, select the namespace of your team and click the **+** button in the top-right corner. Then, copy and paste the following content.

```yaml
kind: KafkaTopic
apiVersion: kafka.strimzi.io/v1beta2
metadata:
  name: warehouse-in
  labels:
    strimzi.io/cluster: warehouse
spec:
  config:
    retention.ms: 604800000
    segment.bytes: 1073741824
```

Then create the KafkaTopic for outcoming traffic by using the OpenShift console in the same way as the previous step.

```yaml
kind: KafkaTopic
apiVersion: kafka.strimzi.io/v1beta2
metadata:
  name: warehouse-out
  labels:
    strimzi.io/cluster: warehouse
spec:
  config:
    retention.ms: 604800000
    segment.bytes: 1073741824
```


