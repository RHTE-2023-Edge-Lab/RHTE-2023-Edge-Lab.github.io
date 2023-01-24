+++
title = "Camel-K Integration"
weight = 8
chapter = true
pre = "<b>Duo C. </b>"
+++

# Deploy the Camel K Integration

In this section, Duo **C** will create the Camel-K Integration.  
The Camel K integration enable us to listen to a new event in the MQTT Broker, transform the body of the message, add a timestamp and finally send the transformed and enriched event to a Kafka Topic.

This step corresponds to the part **3.** of the [global architectural schema](https://rhte-2023-edge-lab.github.io/use-case/architecture/#data-flow):

![Zoom CamelK](/images/schema-zoom-camelk.png)