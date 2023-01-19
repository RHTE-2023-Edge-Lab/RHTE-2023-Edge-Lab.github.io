---
title: "OpenShift"
draft: false
weight: 4
---

## OpenShift cluster

An OpenShift cluster is already deployed via **RHPDS** environment and **"OpenShift 4.11 Workshop"** service.

**No action is required on your side.**

![RHPDS Service](/images/rhpds_service.png)

 The headquarter and warehouses have dedicated namespaces on this OpenShift cluster:

* headquarter
* warehouse-athens
* warehouse-brno
* warehouse-brussels
* warehouse-bucharest
* warehouse-dublin
* warehouse-lisboa
* warehouse-london
* warehouse-paris
* warehouse-stockolm
* warehouse-varsovia

## OpenShift Operators

All the needed operators are already installed on the Openshift cluster.
In the rest of this Lab you will just need to consume the CRD provided by these operators.

* **Red Hat Integration - AMQ Broker for RHEL 8 (Multiarch)**  
AMQ Broker Operator for RHEL 8 (Multiarch) provides the ability to deploy and manage stateful AMQ Broker broker clusters

* **Red Hat Integration - AMQ Streams**  
Red Hat AMQ Streams is a massively scalable, distributed, and high performance data streaming platform based on the Apache Kafka® project. AMQ Streams provides an event streaming backbone that allows microservices and other application components to exchange data with extremely high throughput and low latency. The core capabilities include: * A pub/sub messaging model, similar to a traditional enterprise messaging system, in which application components publish and consume events to/from an ordered stream

- The long term, fault-tolerant storage of events
- The ability for a consumer to replay streams of events
- The ability to partition topics for horizontal scalability

* **Red Hat Integration - Camel K**  
Red Hat Integration - Camel K is a lightweight integration platform, born on Kubernetes, with serverless superpowers.

* **Web Terminal**  
Start a Web Terminal in your browser with common CLI tools for interacting with the cluster. You have access to useful commands for this lab like *oc*, *mosquitto* and *kcat*.  
To launch the web terminal, click the command line terminal icon on the upper right of the console. This instance is automatically logged in with your credentials.

![Openshift Operators](/images/operatos.png)

## OpenShift cluster details

* **OCP Cluster console URL :** `https://console-openshift-console.apps.{{< param openshift_domain >}}/dashboards`

* **OCP Cluster API URL :** `https://api.{{< param openshift_domain >}}:6443`

There is a dedicated OpenShift user for each warehouse.
On your table you will find a poster with the relevant information.
