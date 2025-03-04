---
id: cluster-topology-request
title: "Request cluster topology"
---

This example shows which broker is leader and follower for which partition. This is particularly useful when you run a cluster with multiple Zeebe brokers.

## Related resources

- [Clustering casics](/components/zeebe/technical-concepts/clustering.md)

## Prerequisites

1. Running Zeebe broker with endpoint `localhost:26500` (default)

## TopologyViewer.java

[Source on GitHub](https://github.com/camunda-community-hub/camunda-8-examples/blob/main/zeebe-client-plain-java/src/main/java/io/camunda/zeebe/example/cluster/TopologyViewer.java)

```java
final Topology topology = client.newTopologyRequest().send().join();

System.out.println("Topology:");
topology
    .getBrokers()
    .forEach(
        b -> {
        System.out.println("    " + b.getAddress());
        b.getPartitions()
            .forEach(
                p ->
                    System.out.println(
                        "      " + p.getPartitionId() + " - " + p.getRole()));
        });
```
