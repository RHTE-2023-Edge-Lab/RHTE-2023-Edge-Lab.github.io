---
title: "Architecture"
draft: false
weight: 2
---

## Data flow

![use_case](/images/detailled-schema.svg)

**1.** The ESP8266 scans incoming and outgoing parcels using an RFID scanner. One ESP8266 is dedicated for incoming parcels and one ESP8266 is dedicated for outgoing parcels.  
**2.** The ESP8266 dedicated to incoming parcels writes the RFID tag **identifier** in the **mqtt-in** MQTT topic. The ESP8266 dedicated to outgoing parcels writes the RFID tag **identifier** in the **mqtt-out** MQTT topic.  
**3.** The Camel K operator transforms and enriches MQTT messages stored in MQTT topics and injects those enriched data in the **kafka-in** and **kafka-out** Kafka topics.  
**4.** **Kafka Mirror Maker** mirrors in synchronous mode each topic of each warehouse (10) to the Kafka Broker running in the headquarter in order to centralize data.  
**5.** **Camel Quarkus** aggregates the mirrored topics of each warehouse in a single kafka topic, while enriching each event with metadata.  
**6.** **Kafka Stream** relies on a rocksdb database to retain the last known position of each parcel and send displacement event each time a move is detected. This information is sent in the **shipment** Kafka topic.  
**7.** A web front-end, based on **Quarkus** and connected to the **shipment** kafka topic, displays the moving parcels on a world map along with all warehouses.  

## Red Hat Products

* The MQTT Broker is provided by **Red Hat AMQ Broker**
* The Kafka Broker is provided by **Red Hat AMQ Streams**
* The Camel-K integration is provided by **Red Hat Integration**
* The Kafka Mirror Maker is provided by **Red Hat AMQ Streams**
* Kafka Streams is provided by Quarkus (**Tech Preview**)
* Quarkus is provided by **Red Hat OpenShift Container Platform**

Currently, this lab is hosted on RHPDS because of logistics and expenses considerations but:

* The headquarter could be deployed on a Managed Services offering of OpenShift (**ROSA** or **ARO**)
* The warehouses could be deployed on a **Single Node OpenShift** or **Compact Cluster**