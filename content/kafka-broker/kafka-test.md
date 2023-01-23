---
title: "Check the Kafka broker deployment (duo C)"
draft: false
weight: 4
---

## Kafka pods  

Go to **Workload** > **Pods** and make sure you have similar pods running on your namespace.

![KAFKA pods](/images/warehouse-kafka-pods.png)

## Kafka instances

* Navigate in the OpenShift Administrator console, **Operators** > **Installed Operators**.
* Click on the **Red Hat Integration - AMQ Streams** operator.
* Open the **All instances** tab.
* Select the **Current namespace only** option.
* Make sure you have the following objects:

![KAFKA instances](/images/warehouse-kafka-instances.png)