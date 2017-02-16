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



