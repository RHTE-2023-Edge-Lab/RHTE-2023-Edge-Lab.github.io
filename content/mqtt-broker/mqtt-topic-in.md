---
title: "Create the incoming MQTT Topic (duo A)"
draft: false
weight: 2
---

Create a topic in your MQTT broker by deploying this Custom Resource Definition.

To do so, connect to the OpenShift console, select the namespace of your team and click the **+** button in the top-right corner.
Then, copy and paste the following content.

```yaml
kind: ActiveMQArtemisAddress
apiVersion: broker.amq.io/v1beta1
metadata:
  name: esp8266-in
spec:
  addressName: esp8266-in
  queueName: myQueue0
  routingType: anycast
```
