---
title: "Test the MQTT broker (duo A and B)"
draft: false
weight: 4
---

Go to the OpenShift Administrator console and click the command line terminal icon in the top-right corner to open a **Web Terminal**.
In the Web Terminal, open two tabs: one to send messages, one to receive messages.

On the first terminal, connect to your MQTT Broker to read messages.

```sh
mosquitto_sub -t <MQTT addressName> -h '<AWS LB address for MQTT service>' -p <port> -u <MQTT user> -P <MQTT password>

#Example
#mosquitto_sub -t esp8266-in -h 'a32ba3beef5cf47ba907ca93f911bd94-62424187.eu-west-1.elb.amazonaws.com' -p 1883 -u admin -P public
```

On the second terminal, push new messages to your MQTT Broker.

```sh
for i in {1..200}; do mosquitto_pub -t <MQTT addressName> -h '<AWS LB address for MQTT service>' -p <port> -u <MQTT user> -P <MQTT password> -m in$i; done

#Example
#for i in {1..200}; do mosquitto_pub -t esp8266-in -h 'aa47f83e624da4e639f5be6109757355-1944403232.eu-west-1.elb.amazonaws.com' -p 1883 -u admin -P public -m in$i; done
```

Go back on the first terminal to check if you received messages.

![MQTT topi IN](/images/mqtt-sub-in.png)

![MQTT topi OUT](/images/mqtt-sub-out.png)