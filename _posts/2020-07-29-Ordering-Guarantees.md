---
layout: post
title:  "Ordering Guarantees"
date:   2020-07-29 16:15:05 +0800
categories: jekyll update
---


## Ordering and Causality
* The causal order is not a total order
* Linearizability is stronger than causal consistency
<br>
Causal consistency is the strongest possible consistency model that does not slow down due to network delays, 
and remains available in the face of network failures.

## Sequence Number Ordering
It's easy if there's a single leader
<br>

### Noncausal sequence number generators
* Each node generate its own independent set of sequence numbers
* Use time-of-day clock
* Preallocate blocks of sequence numbers

### Lamport timestamps (provides total ordering)
1. Each node has a unique identifier, and each node keeps a counter of the number of operations it has processed
2. The Lamport timestamp is a pair of (counter, node Id)
3. Every node and every client keeps track of the maximum counter value it has seen so far, and includes that maximum on every request. When a node receives a request or response with a maximum counter value greater than
its own counter value, it immediately increases its own counter to that maximum.

### Timestamp ordering is not sufficient 
unique username example, conflicts can only be detected after the fact.

## Total Order Broadcast
Total order broadcast is usually described as a protocal for exchanging messages between nodes. Informally, it requires that two safety properties always be satisfied:

* Reliable delivery:
<br>
No messages are lost: If a message is delivered to one node, it is delivered to all nodes.

* Totally ordered delivery
Messages are delivered to every node in the same order.

### Implementing linearizable storage using total order broadcast
Use the unique username problem as an example
<br>
1. Append a message to the log, tentatively indicating the username you want to claim.
2. Read the log, and wait for the message you appended to be delivered back to you
3. Check for any message claiming the username that you want. If the first message for your
desired username is your own message, then you are successful: you can commit the username claim
and acknowledge it to the cilent.
While the above procedure ensures linearizable writes, it doesn't guarantee linearizable reads. 
<br> To make reads linearizable, there are a few options:

* You can sequence reads through the log by appending a message, reading the log, and performing 
the actual read when the message is delivered back to you.
* If the log allows you to fetch the position of the latest log message in a linearizable way, you
can query that position, wait for all entries up to that position to be delivered, and then perform the read.
* Read from a synchronously updated replica.

### Implementing total order broadcast using linearizable storage

You have a linearizable register that stores an integer and that has atomic increment-and-get operation. 
for every message, you increment-and-get the linearizable integer, and then attach the value you got to the message
as a sequence number.

However, make a linearizable sequence number generator in a distributed environment needs a consensus algorithm.