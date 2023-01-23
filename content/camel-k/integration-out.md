---
title: "Create Camel K integration to permit MQTT/Kafka transformation and enrich data for outcoming parcels (duo C)"
draft: false
weight: 4
---

Create the camel Integration for the outcoming parcels.  
To do so, connect to the OpenShift console, select the namespace of your team and click the **+** button in the top-right corner. Then, copy and paste the following content.

```yaml
apiVersion: camel.apache.org/v1
kind: Integration
metadata:
  name: kafkaintegration-out
spec:
  sources:
  - content: |
      from("paho:esp8266-out?brokerUrl=tcp://mqtt-mqtt-0-svc:1883&userName=admin&password=public")
      .setHeader("key").simple("${bodyAs(String)}")
      .convertBodyTo(String.class)
      .setBody({ e -> [ parcelNumber: e.in.body, timestamp: new Date().getTime() ] })
      .marshal().json()
      //.to("log:info")
      .to("kamelet:kafka-sink-scram?bootstrapServers=warehouse-kafka-bootstrap:9092&user=camel&password=s3cr3t&topic=warehouse-out")
    name: mqtt-kafka.groovy
  traits: {}
```