---
title: "General"
draft: false
weight: 1
---

## OpenShift Cluster

![RHPDS Service](/images/rhpds_service.png)

OpenShift cluster is already deployed via **RHPDS** environment and **"OpenSHift 4.11 Workshop"** service. The headquarter and warehouses corresponding to different namespace on this Openshift cluster on this way :

* Namespace n°1 :   headquarter
* Namespace n°2 :   warehouse-athens
* Namespace n°3 :   warehouse-brno  
* Namespace n°4 :   warehouse-brussels  
* Namespace n°5 :   warehouse-bucharest
* Namespace n°6 :   warehouse-dublin
* Namespace n°7 :   warehouse-lisboa  
* Namespace n°8 :   warehouse-london  
* Namespace n°9 :   warehouse-paris
* Namespace n°10 :   warehouse-stockolm 
* Namespace n°11 :   warehouse-vasovia

## OpenShift Operators

All needed operators are already installed on the Openshift cluster and you will just need to consume CRD provided by these operators during this session :

* **Red Hat Integration - AMQ Broker for RHEL 8 (Multiarch)**  
AMQ Broker Operator for RHEL 8 (Multiarch) provides the ability to deploy and manage stateful AMQ Broker broker clusters


* **Red Hat Integration - AMQ Streams**  
Red Hat AMQ Streams is a massively scalable, distributed, and high performance data streaming platform based on the Apache Kafka® project. AMQ Streams provides an event streaming backbone that allows microservices and other application components to exchange data with extremely high throughput and low latency. The core capabilities include: * A pub/sub messaging model, similar to a traditional enterprise messaging system, in which application components publish and consume events to/from an ordered stream

- The long term, fault-tolerant storage of events
- The ability for a consumer to replay streams of events
- The ability to partition topics for horizontal scalability

* **Red Hat Integration - Camel K**  
Red Hat Integration - Camel K is a lightweight integration platform, born on Kubernetes, with serverless superpowers.

![Openshift Operators](/images/operatos.png)


