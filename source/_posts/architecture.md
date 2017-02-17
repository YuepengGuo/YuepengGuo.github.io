---
title: architecture
date: 2017-02-16 15:58:09
tags:
---

## microservice architecture

major application components are split up into smaller, separately deployed units, robust, scalability, continuous delivery.

Challenges : correct level of granularity of service components. 

+ too coarse-grained : you may not realize the benefits of this architecture pattern(deployment,scalability,loose coupling)
+ too fine-grained : turn it to service-oriented architecture.

Inter-service communication, undesired coupling, shared database? shared functionality?

since distributed, issues such as contract creation, maintenance, government, availability, authentication and authorization occur.



## event driven architecture

distributed, asynchronous, scalable.

mediator vs broker ? orchestrate multiple steps within an event in central mediator or broker topology to chain events together.

---
event ---> event queue ---> event mediator ---> event channel(s) ---> event processors
---

message queue, or web service endpoint, or any combination thereof.

Event processors are ~~self-contained, independent, highly decoupled~~ architecture components that perform a specific task in the application system. In general, each event-processor component should perform a single business task and not rely on other event processors to complete its specific task.


You should understand variety options and choose solution matches your needs and requirements.

Spring Integration | Apache Camel | BPEL

Broker topology differs from the mediator in that there is no central event mediator, rather, message flow is distributed across the event processor components in a chain-like fashion through a lightweight message broker(activemq,hornetq,etc).

The broker topology is all about ~~the chaining of events to perform a business function~~.




