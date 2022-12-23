+++
title = "Create Content"
date = 2022-12-13T15:22:04+01:00
weight = 5
chapter = true
pre = "<b>X. </b>"
+++

### Chapter 1



# Demo Scenario

In this demo, we will be setting up a lab to explore how Red Hat technologies can assist with edge devices. An edge device, in this context, refers to a device that is located at the edge of a network and is responsible for collecting and processing data from various sources.

We will be using the ESP8266 as our endpoint, which is a low-cost microcontroller with built-in support for WiFi connectivity. This will allow us to connect the endpoint to the internet and send data to our backend systems.

To collect data, we will be using a RFID reader, which is a device that can read and write data to RFID tags. These tags can be attached to various objects, and the RFID reader can detect the presence and information stored on the tags.

Once the data is collected by the endpoint, it will be transmitted to a message broker using the MQTT protocol. A message broker is a software application that enables communication between different devices and systems. MQTT is a lightweight messaging protocol that is well-suited for use in IoT (Internet of Things) applications.

The message broker we will be using is deployed on an OpenShift cluster using the red Hat AMQ operator.

To process the data, we will be using Apache Camel K, which is a lightweight integration framework that allows us to connect different systems and processes together. Camel K will be used to convert the data from MQTT to Apache Kafka, which is a distributed streaming platform that can handle high volumes of data with low latency.

Once the data is in Kafka, we will be using a tool called Mirror Maker to replicate the data from a warehouse to the headquarter. This will allow us to keep the data in sync between the two locations.

Finally, we will be using a frontend user interface to display the data to users. This interface will retrieve data from Kafka and pass it to a Quarkus application, which is a framework for building lightweight, high-performance Java applications.

Overall, this demo will demonstrate how Red Hat technologies can be used to collect and process data from edge devices and make it available to users through a frontend interface.

#TODO : Add schemas here
