---
title: "Final verification (duo C)"
draft: false
weight: 5
---

## Camel Integration

* Navigate in the OpenShift Administrator console, **Operators** > **Installed Operators**.
* Click on the **Red Hat Integration - Camel K** operator.
* Open the **Integration** tab.
* Select the **Current namespace only** option.
* Make sure you have the following objects:

![CamelK integration](/images/camelk-integration.png)

## Kafka User

* Navigate in the OpenShift Administrator console, **Operators** > **Installed Operators**.
* Click on the **Red Hat Integration - AMQ Streams** operator.
* Open the **Kafka User** tab.
* Select the **Current namespace only** option.
* Make sure you have the following object:

![CamelK KafkaUser](/images/camelk-kafka-user.png)