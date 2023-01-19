---
title: "Install the MQTT Broker (duo A or B)"
draft: false
weight: 1
---

Deploy an MQTT broker in your namespace, by deploying this Custom Resource Definition.

To do so, connect to the OpenShift console, select the namespace of your team and click the **+** button in the top-right corner.
Then, copy and paste the following content.

```yaml
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

Now, create a new Kubernetes service to expose your MQTT broker to the internet.
You can use the OpenShift console as used in the previous step.

```yaml
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
