---
layout: post
title:  "Linearizability"
date:   2020-07-29 15:33:05 +0800
categories: jekyll update
---
What is linearizability? The basic idea is make a system appear as if there were only one copy of the data,
and all operations on it are atomic.

## What makes a system linerizable
* Read operation completes before a write operation begins, must return the old value
* Read operation begins after a write operation has completed, must return the new value
* Any read operations that overlap with a write operation, might return either the new or the old value
* After any read has returned the new value, all following reads (on the same or other clients) must return the new value

## Linearizability Versus Serializability
Linerizability is a rencency guarantee on reads and writes of a register (on individual object). It does not group operations into transactions, so it does not prevent problems such as write skew.

A database may provide both serializability and linearizability. Implementations of serilizability based on 2 phase locking or actual serial execution are typically linerizable while serializable snapshot isolation is not linearizable.

## Relying on Linearizability
* Locking and leader election
* Constraints and uniqueness guarantees
* Cross-channel timing dependencies

## Implementing Linearizable Systems

### Single-leader replication (potentially linearizable)
This configuration is linerizable if you reads from the leader or synchronously updated followers. 
<br>
potential issues:
* If a delusional leader continues to serve requests, it is likely to violate linearizability.
* With asychronous replication, failover may loose commited writes, which violates both linearizability and durability

### Consensus algorithms (linearizable)

### Multi-leader replication (not linearizable)

### Leaderless replication (probably not linearizable)
It's possible to make Dynamo-style quorums linearizable at the cost of reduced performance. A reader must perform read repair synchronously, before returning results to the application, and a writer must read the state of a quorum of nodes before sending its writes.

## The cost of linearizability
* When a network fault occurs, you have to choose between either linearizability or total availability
* Linearizability is slow, it's true all the time. If you want linearizability, the response time of read and write is at least proportional to the uncertainty of delays in the network. 