---
title: "Create a Kafka User (duo C)"
draft: false
weight: 2
---

To do so, connect to the OpenShift console, select the namespace of your team and click the **+** button in the top-right corner. Then, copy and paste the following content.

```yaml
kind: Secret
apiVersion: v1
metadata:
  name: mm2-user
stringData:
  password: s3cr3t
type: Opaque
---
apiVersion: kafka.strimzi.io/v1beta2
kind: KafkaUser
metadata:
  labels:
    strimzi.io/cluster: warehouse
  name: mm2
spec:
  authentication:
    password:
      valueFrom:
        secretKeyRef:
          key: password
          name: mm2-user
    type: scram-sha-512
```
