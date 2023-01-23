---
title: "Final verification (duo A and B)"
draft: false
weight: 4
---

## Operator CRD

* Navigate in the OpenShift Administrator console, **Operators** > **Installed Operators**.
* Click on the **AMQ Broker for RHEL 8.x** operator.
* Open the **All instances** tab.
* Select the **Current namespace only** option.
* Make sure you have the following objects:

![MQTT Instances](/images/mqtt-instances.png)

## Pods

Go to **Workload** > **Pods** and make sure the pod **mqtt-ss-0** is present.

![MQTT pods](/images/mqtt-pods.png)

## Services

Go to **Networking** > **Services** and make sure a service named **mqtt-lb** is present.

![MQTT Instances](/images/mqtt-services.png)
