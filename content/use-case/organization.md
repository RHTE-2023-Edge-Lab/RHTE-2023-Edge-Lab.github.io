---
title: "Organization"
date: 2022-12-13T15:22:04+01:00
draft: false
weight: 2
---

During this lab, you will work in pair programming (that is to say: three duo).

* Duo **A** is in charge of implementing the code on ESP8266 to scan the **incoming** parcels using RFID and send the data over MQTT.
* Duo **B** is in charge of implementing the code on ESP8266 to scan the **outgoing** parcels using RFID and send the data over MQTT.
* Duo **C** is in charge of setting up Kafka Broker, MirrorMaker2 and implementing the Camel-K integration.
* Duo **A** or **B** (whoever finishes first) is in charge of deploying the MQTT broker.

## Steps

**1.** Scan entrance and output packets via the ESP8266 RFID scanner. One ESP8266 dedicated for entrance packets and one ESP8266 dedicated for output packets.  
**2.** The ESP8266 dedicated for entrance packets write the **RFID id** in mqtt the **mqtt-in**. The ESP8266 dedicated for output packets write the **RFID id** in the mqtt **mqtt-out**.  
**3.** Camel K operator transform and enrich mqtt data stored in mqtt topics and inject these enriched data in **kafka-in** and **kafka-out** topics.  
**4.** **Kafka Mirror Maker** mirror in synchrone mode each topic of each warehouse (10) to headquarter to centralize data.  
**5.** **Camel Quarkus** help us to have more complex data transformation to aggregate data stored in all regional kafka topics in one and uniq kafka topic (aggregration).  
**6.** **Kafka Stream** relies on a rocksdb database to correlate packet inputs/outputs and finally stores this information in the kafka topic shipment.  
**7.** A web front based on **Quarkus** and connected to the kafka topic **shipment** display data and packages transit on a world map where are represented all warehouses.  


## Schema

![use_case](/images/detailled-schema.svg)

