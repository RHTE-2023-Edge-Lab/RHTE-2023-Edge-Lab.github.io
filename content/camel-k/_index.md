+++
title = "Camel-K Integration"
weight = 6
chapter = true
pre = "<b>C. </b>"
+++

# Deploy Camel K integration

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

## 

## Duo C - Create needed Kamelet to able Camel K to connect to our Kafka

```sh
apiVersion: camel.apache.org/v1alpha1
kind: Kamelet
metadata:
  name: kafka-sink-scram
  namespace: <your_namespace>
  annotations:
    camel.apache.org/kamelet.support.level: "Stable"
    camel.apache.org/catalog.version: "main-SNAPSHOT"
    camel.apache.org/kamelet.icon: "data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiBBZG9iZSBJbGx1c3RyYXRvciAxOS4wLjAsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW9uOiA2LjAwIEJ1aWxkIDApICAtLT4NCjxzdmcgdmVyc2lvbj0iMS4xIiBpZD0iTGF5ZXJfMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIgeD0iMHB4IiB5PSIwcHgiDQoJIHZpZXdCb3g9IjAgMCA1MDAgNTAwIiBzdHlsZT0iZW5hYmxlLWJhY2tncm91bmQ6bmV3IDAgMCA1MDAgNTAwOyIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI+DQo8ZyBpZD0iWE1MSURfMV8iPg0KCTxwYXRoIGlkPSJYTUxJRF85XyIgZD0iTTMxNC44LDI2OS43Yy0xNC4yLDAtMjcsNi4zLTM1LjcsMTYuMkwyNTYuOCwyNzBjMi40LTYuNSwzLjctMTMuNiwzLjctMjAuOWMwLTcuMi0xLjMtMTQuMS0zLjYtMjAuNg0KCQlsMjIuMy0xNS43YzguNyw5LjksMjEuNCwxNi4xLDM1LjYsMTYuMWMyNi4yLDAsNDcuNi0yMS4zLDQ3LjYtNDcuNnMtMjEuMy00Ny42LTQ3LjYtNDcuNnMtNDcuNiwyMS4zLTQ3LjYsNDcuNg0KCQljMCw0LjcsMC43LDkuMiwyLDEzLjVsLTIyLjMsMTUuN2MtOS4zLTExLjYtMjIuOC0xOS42LTM4LjEtMjIuMXYtMjYuOWMyMS42LTQuNSwzNy44LTIzLjcsMzcuOC00Ni42YzAtMjYuMi0yMS4zLTQ3LjYtNDcuNi00Ny42DQoJCWMtMjYuMiwwLTQ3LjYsMjEuMy00Ny42LDQ3LjZjMCwyMi42LDE1LjgsNDEuNSwzNi45LDQ2LjN2MjcuM2MtMjguOCw1LjEtNTAuOCwzMC4yLTUwLjgsNjAuNWMwLDMwLjQsMjIuMiw1NS43LDUxLjIsNjAuNXYyOC44DQoJCWMtMjEuMyw0LjctMzcuNCwyMy43LTM3LjQsNDYuNGMwLDI2LjIsMjEuMyw0Ny42LDQ3LjYsNDcuNmMyNi4yLDAsNDcuNi0yMS4zLDQ3LjYtNDcuNmMwLTIyLjctMTYtNDEuOC0zNy40LTQ2LjR2LTI4LjgNCgkJYzE1LTIuNSwyOC4yLTEwLjQsMzcuNC0yMS44bDIyLjUsMTUuOWMtMS4yLDQuMy0xLjksOC43LTEuOSwxMy40YzAsMjYuMiwyMS4zLDQ3LjYsNDcuNiw0Ny42czQ3LjYtMjEuMyw0Ny42LTQ3LjYNCgkJQzM2Mi40LDI5MSwzNDEuMSwyNjkuNywzMTQuOCwyNjkuN3ogTTMxNC44LDE1OC40YzEyLjcsMCwyMy4xLDEwLjQsMjMuMSwyMy4xYzAsMTIuNy0xMC4zLDIzLjEtMjMuMSwyMy4xcy0yMy4xLTEwLjQtMjMuMS0yMy4xDQoJCUMyOTEuOCwxNjguOCwzMDIuMSwxNTguNCwzMTQuOCwxNTguNHogTTE3NiwxMTUuMWMwLTEyLjcsMTAuMy0yMy4xLDIzLjEtMjMuMWMxMi43LDAsMjMuMSwxMC40LDIzLjEsMjMuMQ0KCQljMCwxMi43LTEwLjMsMjMuMS0yMy4xLDIzLjFDMTg2LjMsMTM4LjIsMTc2LDEyNy44LDE3NiwxMTUuMXogTTIyMi4xLDM4NC45YzAsMTIuNy0xMC4zLDIzLjEtMjMuMSwyMy4xDQoJCWMtMTIuNywwLTIzLjEtMTAuNC0yMy4xLTIzLjFjMC0xMi43LDEwLjMtMjMuMSwyMy4xLTIzLjFDMjExLjgsMzYxLjgsMjIyLjEsMzcyLjIsMjIyLjEsMzg0Ljl6IE0xOTkuMSwyODEuMw0KCQljLTE3LjcsMC0zMi4yLTE0LjQtMzIuMi0zMi4yYzAtMTcuNywxNC40LTMyLjIsMzIuMi0zMi4yYzE3LjcsMCwzMi4yLDE0LjQsMzIuMiwzMi4yQzIzMS4yLDI2Ni45LDIxNi44LDI4MS4zLDE5OS4xLDI4MS4zeg0KCQkgTTMxNC44LDM0MC4zYy0xMi43LDAtMjMuMS0xMC40LTIzLjEtMjMuMWMwLTEyLjcsMTAuMy0yMy4xLDIzLjEtMjMuMXMyMy4xLDEwLjQsMjMuMSwyMy4xQzMzNy45LDMzMCwzMjcuNSwzNDAuMywzMTQuOCwzNDAuM3oiLz4NCjwvZz4NCjwvc3ZnPg0K"
    camel.apache.org/provider: "Apache Software Foundation"
    camel.apache.org/kamelet.group: "Kafka"
  labels:
    camel.apache.org/kamelet.type: "sink"
spec:
  definition:
    title: "Kafka Sink (SCRAM Authentication)"
    description: |-
      Send data to Kafka topics.

      The Kamelet is able to understand the following headers to be set:

      - `key` / `ce-key`: as message key
    
      - `partition-key` / `ce-partitionkey`: as message partition key

      Both the headers are optional.
    required:
      - topic
      - bootstrapServers
      - user
      - password
    type: object
    properties:
      topic:
        title: Topic Names
        description: Comma separated list of Kafka topic names
        type: string
      bootstrapServers:
        title: Bootstrap Servers
        description: Comma separated list of Kafka Broker URLs
        type: string
      securityProtocol:
        title: Security Protocol
        description: Protocol used to communicate with brokers. SASL_PLAINTEXT, PLAINTEXT, SASL_SSL and SSL are supported
        type: string
        default: SASL_PLAINTEXT
      saslMechanism:
        title: SASL Mechanism
        description: The Simple Authentication and Security Layer (SASL) Mechanism used. 
        type: string
        default: SCRAM-SHA-512
      user:
        title: Username
        description: Username to authenticate to Kafka 
        type: string
        x-descriptors:
        - urn:camel:group:credentials
      password:
        title: Password
        description: Password to authenticate to kafka
        type: string
        format: password
        x-descriptors:
        - urn:alm:descriptor:com.tectonic.ui:password
        - urn:camel:group:credentials
  dependencies:
    - "camel:core"
    - "camel:kafka"
    - "camel:kamelet"
  template:
    from:
      uri: "kamelet:source"
      steps:
      - choice:
          when:
          - simple: "${header[key]}"
            steps:
            - set-header:
                name: kafka.KEY
                simple: "${header[key]}"
          - simple: "${header[ce-key]}"
            steps:
            - set-header:
                name: kafka.KEY
                simple: "${header[ce-key]}"
      - choice:
          when:
          - simple: "${header[partition-key]}"
            steps:
            - set-header:
                name: kafka.PARTITION_KEY
                simple: "${header[partition-key]}"
          - simple: "${header[ce-partitionkey]}"
            steps:
            - set-header:
                name: kafka.PARTITION_KEY
                simple: "${header[ce-partitionkey]}"
      - to:
          uri: "kafka:{{topic}}"
          parameters:
            brokers: "{{bootstrapServers}}"
            securityProtocol: "{{securityProtocol}}"
            saslMechanism: "{{saslMechanism}}"
            saslJaasConfig: "org.apache.kafka.common.security.scram.ScramLoginModule required username='{{user}}' password='{{password}}';"
```

## Duo C - Create Camel K integration to permit MQTT/Kafka transformation and enrich data for entrance packets

```sh
apiVersion: camel.apache.org/v1
kind: Integration
metadata:
  name: kafkaintegration-in
  namespace: <your_namespace>
spec:
  sources:
  - content: |
      from("paho:esp8266-in?brokerUrl=tcp://mqtt-mqtt-0-svc:1883&userName=admin&password=public")
      .setHeader("kafka.KEY").simple("${body}")
      .convertBodyTo(String.class)
      .setBody({ e -> [ parcelnumber: e.in.body, timestamp: new Date().getTime() ] })
      .marshal().json()
      //.to("log:info")
      .to("kamelet:kafka-sink-scram?bootstrapServers=warehouse-kafka-bootstrap:9092&user=camel&password=s3cr3t&topic=warehouse-in")
    name: mqtt-kafka.groovy
  traits: {}
```

## Duo C - Create Camel K integration to permit MQTT/Kafka transformation and enrich data for output packets

```sh
apiVersion: camel.apache.org/v1
kind: Integration
metadata:
  name: kafkaintegration-out
  namespace: <your_namespace>
spec:
  sources:
  - content: |
      from("paho:esp8266-out?brokerUrl=tcp://mqtt-mqtt-0-svc:1883&userName=admin&password=public")
      .setHeader("kafka.KEY").simple("${body}")
      .convertBodyTo(String.class)
      .setBody({ e -> [ parcelNumber: e.in.body, timestamp: new Date().getTime() ] })
      .marshal().json()
      //.to("log:info")
      .to("kamelet:kafka-sink-scram?bootstrapServers=warehouse-kafka-bootstrap:9092&user=camel&password=s3cr3t&topic=warehouse-out")
    name: mqtt-kafka.groovy
  traits: {}
```

## Result

![CamelK integration](/images/camelk-integration.png)

## Check your Kafka topic with the enriched data

1. Connect to one of your kafka pod  

```sh
oc rsh -n <project> <cluster-name>-kafka-0
```

2. Create a client.properties file  

```sh
cat <<EOF>> /tmp/client.properties
sasl.mechanism=SCRAM-SHA-512
security.protocol=SASL_PLAINTEXT
sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required \
   username="<user>" \
   password="<password>";
EOF
```

3. Connect to your Kafka topic to see your messages 

```sh
./bin/kafka-console-consumer.sh --bootstrap-server <kafka-bootstrap-service>:9092 --topic <your-topic> --consumer.config /tmp/client.properties --from-beginning
```

![Result](/images/result.png)