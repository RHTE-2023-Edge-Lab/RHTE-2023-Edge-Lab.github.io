---
title: "Test the MQTT broker (duo A and B)"
draft: false
weight: 4
---

Go to the OpenShift Administrator console and click the command line terminal icon in the top-right corner to open a **Web Terminal**.
In the Web Terminal, open two tabs: one to send messages, one to receive messages.

On the first terminal, connect to your MQTT Broker to read messages. On the commands bellow change \<VAR\> with the correct values. See details and an example in comments.

```sh
# First retrieve the url of the load balancer created when you deployed the MQTT service :
export AWS_LB_URL = $(oc get svc -n warehouse-<CITY> -ojsonpath="{.status.loadBalancer.ingress[0].hostname}")

mosquitto_sub -t <MQTT addressName> -h ${AWS_LB_URL} -p <port> -u <MQTT user> -P <MQTT password>

# Where :

# <CITY> is the name of the team city
# <MQTT addressName> is the addressName of the ActiveMQArtemisAddress custom resource (CR) you deployed
# <port> the port defined in the ActiveMQArtemis CR
# <MQTT user> the adminUser defined in the ActiveMQArtemis CR
# <MQTT password> the adminPassword defined in the ActiveMQArtemis CR

# Example :

# mosquitto_sub -t esp8266-in -h 'a32ba3beef5cf47ba907ca93f911bd94-62424187.eu-west-1.elb.amazonaws.com' -p 1883 -u admin -P public
```

On the second terminal, push new messages to your MQTT Broker. On the commands bellow change \<VAR\> with the correct values. See details and an example in comments.

```sh
# First retrieve the url of the load balancer created when you deployed the MQTT service :
export AWS_LB_URL = $(oc get svc -n warehouse-<CITY> -ojsonpath="{.status.loadBalancer.ingress[0].hostname}")

for i in {1..200}; do mosquitto_pub -t <MQTT addressName> -h ${AWS_LB_URL} -p <port> -u <MQTT user> -P <MQTT password> -m in$i; done

# Where :

# <CITY> is the name of the team city
# <MQTT addressName> is the addressName of the ActiveMQArtemisAddress custom resource (CR) you deployed
# <port> the port defined in the ActiveMQArtemis CR
# <MQTT user> the adminUser defined in the ActiveMQArtemis CR
# <MQTT password> the adminPassword defined in the ActiveMQArtemis CR

# Example :

# for i in {1..200}; do mosquitto_pub -t esp8266-in -h 'aa47f83e624da4e639f5be6109757355-1944403232.eu-west-1.elb.amazonaws.com' -p 1883 -u admin -P public -m in$i; done
```

Go back on the first terminal to check if you received messages.

![MQTT topi IN](/images/mqtt-sub-in.png)

![MQTT topi OUT](/images/mqtt-sub-out.png)