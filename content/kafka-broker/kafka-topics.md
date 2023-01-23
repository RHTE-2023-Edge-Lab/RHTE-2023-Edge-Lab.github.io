---
title: "Create the incoming and outcoming Kafka Topics (duo C)"
draft: false
weight: 3
---

Create the incoming and outcoming Kafka Topics by deploying these CRDs.

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
---
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


