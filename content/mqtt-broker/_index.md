+++
title = "MQTT Broker"
weight = 3
chapter = true
pre = "<b>A & B. </b>"
+++

### Duo A & B

# Deploy the MQTT Broker in your namespace

```sh
apiVersion: broker.amq.io/v1beta1
kind: ActiveMQArtemis
metadata:
  name: mqtt
  namespace: <your_namespace>
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

# Deploy an AMQ Broker Address for your MQTT Broker

```sh
kind: ActiveMQArtemisAddress
apiVersion: broker.amq.io/v1beta1
metadata:
  name: address
  namespace: <your_namespace>
spec:
  addressName: esp8266
  queueName: myQueue0
  routingType: anycast
```

# Deploy your Service in LoadBalancer type   

```sh
apiVersion: v1
kind: Service
metadata:
  name: mqtt-lb
  namespace: <your_namespace>
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

# Result

## MQTT Instances
![MQTT Instances](/images/mqtt-instances.png)

## MQTT Pods
![MQTT pods](/images/mqtt-pods.png)

## MQTT Services
![MQTT Instances](/images/mqtt-services.png)