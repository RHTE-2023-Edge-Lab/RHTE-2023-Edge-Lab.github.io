+++
title = "MirrorMaker2"
weight = 7
chapter = true
pre = "<b>C. </b>"
+++

# Configure MirrorMaker2

## Duo C - Prepare TLS communication
Mirrormaker2 will use TLS using his own certificate to communicate from the warehouse to the headquarter.
Create the secret with the ca.crt from the headquarter kafka cluster.

```sh
oc get secret headquarter-clients-ca-cert -n headquarter -o jsonpath="{.data.ca\.crt}" | base64 -d > /tmp/ca.crt
oc create secret generic headquarter-ca --from-file /tmp/ca.crt -n your_namespace
```

## Duo C - Create secret 
```sh
apiVersion: v1
stringData:
  password: <your_city>
kind: Secret
metadata:
  name: headquarter-<your_city>
  namespace: <your_namespace>
type: Opaque
```

## Duo C - Deploy MirrorMaker2

You need to get the bootstrap route of the kafka's headquarter cluster 

```sh
oc get route headquarter-kafka-tls-bootstrap -n headquarter
```

Following manifest must be updated with your values :
```sh
apiVersion: kafka.strimzi.io/v1beta2
kind: KafkaMirrorMaker2
metadata:
  name: mirrormaker2
  namespace: your_namespace
spec:
  clusters:
    - alias: headquarter
      authentication:
        passwordSecret:
          password: password
          secretName: headquarter-<your city>
        type: scram-sha-512
        username: warehouse-<your city>
      bootstrapServers: '<BOOTSTRAP HEADQUARTER ROUTE>:443'
      tls:
        trustedCertificates:
          - certificate: ca.crt
            secretName: headquarter-ca
    - alias: warehouse
      authentication:
        passwordSecret:
          password: password
          secretName: camel-user
        type: scram-sha-512
        username: camel
      bootstrapServers: 'warehouse-kafka-bootstrap:9092'
      config:
        config.storage.replication.factor: -1
        offset.storage.replication.factor: -1
        status.storage.replication.factor: -1
  connectCluster: headquarter
  mirrors:
    - checkpointConnector:
        config:
          checkpoints.topic.replication.factor: 1
      groupsPattern: .*
      heartbeatConnector:
        config:
          heartbeats.topic.replication.factor: 1
      sourceCluster: warehouse
      sourceConnector:
        config:
          offset-syncs.topic.replication.factor: 1
          replication.factor: 1
          sync.topic.acls.enabled: 'false'
      targetCluster: headquarter
      topicsPattern: "warehouse-in,warehouse-out"
  replicas: 1
  version: 3.2.3
```
