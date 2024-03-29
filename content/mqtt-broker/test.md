---
title: "Test the MQTT broker (duo A and B)"
draft: false
weight: 4
---

Go to the OpenShift Administrator console and click the command line terminal icon **>_** in the top-right corner to open a **Web Terminal**.
In the Web Terminal, open two tabs: one to send messages, one to receive messages.

On the first terminal, connect to your MQTT Broker to read incoming messages.

```sh
# Get the load balancer URL generated from the service created
LOAD_BALANCER_URL=$(oc get svc mqtt-lb -ojsonpath="{.status.loadBalancer.ingress[0].hostname}")
# Subscribe to MQTT topic
mosquitto_sub -t esp8266-in -h ${LOAD_BALANCER_URL} -p 1883 -u admin -P public
```

On the second terminal, push new messages to your MQTT Broker.

```sh
# Get the load balancer URL generated from the service created
LOAD_BALANCER_URL=$(oc get svc mqtt-lb -ojsonpath="{.status.loadBalancer.ingress[0].hostname}")
# Subscribe to MQTT topic
for i in {1..200}; do mosquitto_pub -t esp8266-in -h ${LOAD_BALANCER_URL} -p 1883 -u admin -P public -m in$i; done
```

Go back on the first terminal to check if you received messages.

![MQTT topi IN](/images/mqtt-sub-in.png)

Press CTRL+C on the first terminal and do the same process to check your MQTT Broker for outcoming messages:  
On the first terminal, connect to your MQTT Broker to read outcoming messages.

```sh
# Get the load balancer URL generated from the service created
LOAD_BALANCER_URL=$(oc get svc mqtt-lb -ojsonpath="{.status.loadBalancer.ingress[0].hostname}")
# Subscribe to MQTT topic
mosquitto_sub -t esp8266-out -h ${LOAD_BALANCER_URL} -p 1883 -u admin -P public
```

On the second terminal, push new messages to your MQTT Broker.

```sh
# Get the load balancer URL generated from the service created
LOAD_BALANCER_URL=$(oc get svc mqtt-lb -ojsonpath="{.status.loadBalancer.ingress[0].hostname}")
# Subscribe to MQTT topic
for i in {1..200}; do mosquitto_pub -t esp8266-out -h ${LOAD_BALANCER_URL} -p 1883 -u admin -P public -m out$i; done
```

Go back on the first terminal to check if you received messages.

![MQTT topi OUT](/images/mqtt-sub-out.png)