+++
title=  "ZooKeeper notes"
date=   2021-07-14 14:15:00
+++

# ZooKeeper

## Key Words:
* group messaging
* shared registers
* distributed lock service
* wait-free
* manipulates simple wait-free data objects organized hierarchically as in file systems
* Zab (leader-based atomic broadcast protocol) to provide linearizability
* relaxed consistency guarantees
* watch
* A-linearizability
## Data model
* file system
## Sessions
* Sessions enable a client to move transparently from one server to another within a ZooKeeper ensemble, and hence persist across ZooKeeper servers

## Services can be implemented
* Lock with/without herd effect
* Read/Write Locks
* Double Barrier
* Configuration Management
* Group Membership
* Rendezvous
*
## Node Types
* regular
* ephemeral
* sequential
## Client API
* create(path, data, flags)
* delete(path, version)
* exists(path, watch)
* getData(path, watch)
* setData(path, data, version)
* getChildren(path, watch)
* sync(path)
## ZooKeeper guarantees
### Linearizable writes (A-linearizability)
* all requests that update the state of ZooKeeper are serializable and respect precedence
### FIFO client order
* all requests from a given client are exeuted in the order that they were sent by the client


