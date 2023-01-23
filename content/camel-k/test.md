---
title: "Test the Camel K Integrations (duo C)"
draft: false
weight: 6
---

Go to the OpenShift Administrator console and click the command line terminal icon **>_** in the top-right corner to open a **Web Terminal**.  
Copy and paste the following commands to retrieve events from your Kafka topics.

1. Connect to one of your kafka pod  

```sh
oc rsh warehouse-kafka-0
```

2. Create a client.properties file  

```sh
cat <<EOF> /tmp/client.properties
sasl.mechanism=SCRAM-SHA-512
security.protocol=SASL_PLAINTEXT
sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required \
   username="mm2" \
   password="s3cr3t";
EOF
```

3. Connect to your warehouse-in Kafka topic to see your incoming parcels. Scan the parcel RFID with your ESP8266 used for incoming parcels.

```sh
./bin/kafka-console-consumer.sh --bootstrap-server warehouse-kafka-bootstrap:9092 --topic warehouse-in --consumer.config /tmp/client.properties --from-beginning
```

4. Press CTRL + C and connect now to your warehouse-out Kafka topic to see your outcoming parcels. Scan the parcel RFID with your ESP8266 used for outcoming parcels.

```sh
./bin/kafka-console-consumer.sh --bootstrap-server warehouse-kafka-bootstrap:9092 --topic warehouse-out --consumer.config /tmp/client.properties --from-beginning
```

You should see similar results as the followings:

![Result](/images/camelk-topic-result.png)