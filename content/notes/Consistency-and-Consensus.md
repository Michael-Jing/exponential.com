+++
title=  "Consistency and Consensus"
date=   2021-07-22 11:04:00
+++

# Consistency Guarantees
## Eventual consistency (convergence)
## Linearizability (make a system appear as if there were only one copy of the data, and all operations on it are atomic)
### example - alice has seen the result of a game, while later bob sees that they are still playing, because they are reading from 2 different replicas
### What Makes a System Linearizable
* If a read request is concurrent with a write request, it may return either the old or the new value.
* After any one read has returned the new value, all following reads (on the same or other clients) must also return the new value.
* The requirement of linearizability is that the lines joining up the operation markers always move forward in time (from left to right), never backward. 
### Linearizability Versus Serializability
* Serializability is an isolation nproperty of transacitons, where every transaction may read and write multiple objects
* Linearizability is a recency guarantee on reads and writes of a register (an individual object). 
* A database may provide both serializability and linearizability, and this combination is known as strict serializability or strong one-copy serializability. 
* Implementations of serializability based on two-phase locking or actual serial execution are typically linearizable.
### Relying on Linearizability
#### Locking and leader election

#### Constraints and uniqueness guarantees