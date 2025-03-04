---
id: what-is-camunda-cloud
title: "What is Camunda Cloud?"
description: "Camunda Cloud is a software as a service (SaaS) offering for microservice orchestration."
---

[Camunda Cloud](https://camunda.io) is a **software as a service** (**SaaS**) offering for microservice orchestration. Camunda Cloud ensures that, once started, flows are always carried out fully, retrying steps in case of failures. Along the way, Camunda Cloud maintains a complete audit log so the progress of flows can be tracked.

Camunda Cloud is a turn-key solution that accelerates your project implementation. It is particularly suited for integrating heterogeneous systems and orchestrating dispersed services implemented in different programming languages.

Behind the scenes, resources inside Camunda Cloud are highly available and fault tolerant to give you peace of mind. Finally, Camunda Cloud offers clusters of different sizes, so you can pick the offering that best fits your business needs.

## What problem does it solve?

A company’s end-to-end processes almost always span more than one microservice. In an e-commerce company, for example, a “customer order” process might involve a payments microservice, an inventory microservice, a shipping microservice, and more:

![order-process](assets/order-process.png)

These end-to-end processes are mission critical, yet the processes themselves are rarely modeled and monitored. Often, the flow of events through different microservices is not as transparent as it should be.

Camunda Cloud addresses these challenges and provides:

1. **Visibility** into the state of a company’s end-to-end processes, including the number of in-flight processes, average process duration, current errors within a process, and more.
2. **Process orchestration** based on the current state of a process; Camunda Cloud publishes “jobs” that can be executed by one or more microservices, ensuring the progress of processes according to their definition.
3. **Monitoring for timeouts** or other process errors with the ability to configure error-handling steps such as stateful retries or escalation to teams that can resolve an issue manually.

## What are its core quality attributes?

Camunda Cloud is designed to operate on a very large scale. To achieve this, it provides:

- **Horizontal scalability** and no dependence on an external database; Zeebe (the workflow engine inside Camunda Cloud) writes data directly to the filesystem on the same servers where it is deployed. Zeebe makes it simple to distribute processing across a cluster of machines to deliver high throughput.
- **High availability and fault tolerance** via a pre-configured replication mechanism, ensuring Camunda Cloud can recover from machine or software failure with no data loss and minimal downtime. This ensures the system as a whole remains available without requiring manual action.
- **Audit trail** as all process-relevant events are written to an append-only log, providing an audit trail and a history of the state of a process.
- **Reactive publish-subscribe interaction model** which enables microservices that connect to Camunda Cloud to maintain a high degree of control and autonomy, including control over processing rates. These properties make Camunda Cloud resilient, scalable, and reactive.
- **Visual processes modeled in ISO-standard BPMN 2.0** so technical and non-technical stakeholders can collaborate on process design in a widely-used modeling language.
- **Language-agnostic client model** makes it possible to build a client in nearly any programming language an organization uses to build microservices.
- **Operational ease-of-use** as a SaaS provider we take care of all operational details.

## How does it compare to other solutions?

Most existing workflow engines offer a vast amount of features. While having access to lots of features is generally a good thing, it can come at a cost of increased complexity and degraded performance.

Camunda Cloud is entirely focused on providing a compact, robust, and scalable solution for process orchestration. Rather than supporting a broad spectrum of features, its goal is to excel within this scope.
