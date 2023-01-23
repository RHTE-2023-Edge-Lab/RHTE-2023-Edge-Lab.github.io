---
title: "Create a Kafka User (duo C)"
draft: false
weight: 1
---
Create a Kafka User for Camel K Integration.


Fist create a secret containing the Kafka User password.  
To do so, connect to the OpenShift console, select the namespace of your team and click the **+** button in the top-right corner. Then, copy and paste the following content.

```yaml
kind: Secret
apiVersion: v1
metadata:
  name: camel-user
stringData:
  password: s3cr3t
type: Opaque
```

Then create the Kafka user for the Camel K Integration. You can use the OpenShift console as used in the previous step.

```yaml
apiVersion: kafka.strimzi.io/v1beta2
kind: KafkaUser
metadata:
  name: camel
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