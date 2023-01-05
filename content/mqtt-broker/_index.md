+++
title = "MQTT Broker"
weight = 3
chapter = true
pre = "<b>A & B. </b>"
+++


# MQTT Broker in each warehouse

## Duo A & B - Deploy your MQTT Broker in your dedicated namespace corresponding to your warehouse

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

## Duo A & B - Deploy your Service in LoadBalancer type   

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


## Duo A - Deploy your AMQ Broker Address for your MQTT topic IN for entrance packets

```sh
kind: ActiveMQArtemisAddress
apiVersion: broker.amq.io/v1beta1
metadata:
  name: esp8266-in
  namespace: <your_namespace>
spec:
  addressName: esp8266-in
  queueName: myQueue0
  routingType: anycast
```

## Duo B - Deploy your AMQ Broker Address for your MQTT topic OUT for output packets

```sh
kind: ActiveMQArtemisAddress
apiVersion: broker.amq.io/v1beta1
metadata:
  name: esp8266-out
  namespace: <your_namespace>
spec:
  addressName: esp8266-out
  queueName: myQueue0
  routingType: anycast
```


## Result

### MQTT Instances
![MQTT Instances](/images/mqtt-instances.png)

### MQTT Pods
![MQTT pods](/images/mqtt-pods.png)

### MQTT Services
![MQTT Instances](/images/mqtt-services.png)



## Test your MQTT deployment

1. Connect to your MQTT Broker to read messages

```sh
mosquitto_sub -t <MQTT addressName> -h '<AWS LB address for MQTT service>' -p <port> -u <MQTT user> -P <MQTT password>

#Example
#mosquitto_sub -t esp32 -h 'a32ba3beef5cf47ba907ca93f911bd94-62424187.eu-west-1.elb.amazonaws.com' -p 1883 -u admin -P public
```


2. Push new messages to your MQTT Broker

```sh
for i in {1..200}; do mosquitto_pub -t <MQTT addressName> -h '<AWS LB address for MQTT service>' -p <port> -u <MQTT user> -P <MQTT password> -m message$i; done

#Example
#for i in {1..200}; do mosquitto_pub -t esp32 -h 'aa47f83e624da4e639f5be6109757355-1944403232.eu-west-1.elb.amazonaws.com' -p 1883 -u admin -P public -m message$i; done
```



![MQTT test](/images/mqtt-sub.png)
