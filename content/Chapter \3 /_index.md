+++
title = "Create Content"
date = 2022-12-13T15:22:04+01:00
weight = 5
chapter = true
pre = "<b>X. </b>"
+++

### Chapter 3


### Define an address in the Artemis broker

The first step in this configuration is to define an address in the Artemis broker that will be used to route messages. This is done using the ActiveMQArtemisAddress resource. The address is called "esp32" and is configured to use an "anycast" routing type, which means that messages sent to this address will be delivered to a single consumer. The address also specifies a queue called "myQueue0" that will be used to store the messages.

```shell
kind: ActiveMQArtemisAddress
apiVersion: broker.amq.io/v1beta1
metadata:
  name: address
spec:
  addressName: esp32
  queueName: myQueue0
  routingType: anycast
```

### Define the Artemis broker

The next step is to define the Artemis broker itself. This is done using the ActiveMQArtemis resource. The configuration specifies the acceptors that the broker will use to listen for incoming connections, as well as the administrator user and password that will be used to access the broker's management console. The configuration also specifies a deployment plan for the broker, which includes details such as the size of the cluster, the type of journal to use, and whether or not persistence is enabled.

```shell
apiVersion: broker.amq.io/v1beta1
kind: ActiveMQArtemis
metadata:
  name: mqtt
spec:
  acceptors:
  - connectionsAllowed: 5
    expose: true
    name: mqtt
    port: 1883
    protocols: mqtt
    sslEnabled: false
  adminPassword: public
  adminUser: admin
  console:
    expose: true
  deploymentPlan:
    image: placeholder
    jolokiaAgentEnabled: false
    journalType: nio
    managementRBACEnabled: true
    messageMigration: false
    persistenceEnabled: false
    requireLogin: true
    size: 1
```


### Expose the Artemis broker to external clients

Finally, we need to expose the Artemis broker to external clients. This is done using a Kubernetes Service, which listens on port 1883 (the default port for MQTT) and routes traffic to the Artemis pods that are running on the cluster. The Service also uses a load balancer to distribute traffic.

To create the Service, we will use the following configuration:

```shell
apiVersion: v1
kind: Service
metadata:
  name: mqtt-lb
spec:
  ports:
  - name: mqtt-lb
    port: 1883
    protocol: TCP
    targetPort: 1883
  selector:
    ActiveMQArtemis: mqtt
    application: mqtt-app
  type: LoadBalancer
```