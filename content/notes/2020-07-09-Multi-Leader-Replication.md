+++
title=  "Multi-Leader Replication"
date=   2020-07-09 16:55:05
+++
A natural extension of the leader-based replication model is to allow more than one node to accepts, which requires Multi-Leader Replication. Within each datacenter, regular leader-follower replication is used; between datacenters, each datacenter's leader replicates
its changes to the leaders in other datacenters.

## Use Cases for Multi-Leader Replication
* Multi-datacenter operation
    * Inter-datacenter network delay is hidden from users, thus better perceived performance.
    * Tolerance of datacenter outages
    * Tolerance of network problems (traffic between datacenters usually goes over the public internet)
* Clients with offline operation
* Collaborative-editing

## Handling Write Conflicts
* Conflict avoidance (writes for a particular record go through the same leader)
* Converging toward a consistent state
    * Give each write a unique ID
    * Give each replica a unique ID
    * Merge the result together (with application code to resolve the conflict or not)
* Custom conflict resolution logic
    * On Write, typically runs in background
    * On Read, the application may prompt the user or automatically resolve the conflict and write the result back to db.
## Automatic Conflict Resolution
* Conflict-free replicated datatypes
* Mergeable persisitent data structures
* Oporation transformation
## Multi-Leader Replication Topologies
* Circular topology (single point of failure)
* Star topology (single point of failure)
* All-to-all topology (Dependent update arrives before insert issue, use version vectors to solve)
