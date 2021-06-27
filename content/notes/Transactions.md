+++
title=  "Transactions"
date=   2021-06-12 20:49:00
+++

Transactions

# ACID

## Atomicity
* Abortability, implemented using write ahead log

## Consistency
* Satisfy application defined invariant
## Isolation
* Different isolation levels

## Durability

# Atomic operations on single object
* compare and set
* atomic increment

# Isolation Levels
## Read committed
### Properties
* no dirty reads (only read commited result)
* no dirty writes (only override previously commited record)
	* issues if dirty writes (Bob buy car but invoice sent to alice)

### Implementation
* use locks to prevent concurrent writes
* keep old value during writes, and return old value if there's a request

## Snapshot Isolation & Repeatable Read
### Properties

### Implementation
* MVCC
	* unique monotonically increasing transaction id
	* visibility rule
		* At the time when the reader's transaction started, the transaction that created the object had already committed.
		* The object is not marked for deletion, or if it is, the transaction that requested deletion had not yet committed at the time when the reader's transaction started.
### Indexes and snapshot isolation

* index points to all versions of an object
* append-only/copy-on-write
* every write transaction (or batch of transactions) creates a new B-tree root, and a particular root is a consistent snapshot of the database at the point in time when it was created.

## Serializability
### How to implement
* Literally exectute transactions in a serial order
	* every transaction must be small and fast
	* limited to use cases where active data can fit in memory
	* write throughput must be low enough to be handled on a single cpu core, or else
	transactions need to be partitioned without requiring cross-partition coordination
	* cross-partition transactions are possible, but there's a hard limit to the extent to which they can be used

* Two-phase locking
	* In 2PL, writers don't just block other writers; they also block readers and vice versa

* optimistic concurrency control techniques
	* Detecting stale MVCC reads
		* the database needs to track when a transaction ignores another transaction's writes due to MVCC visibility rules. When the transaction wants to commit, the database checks whether any of the ignored writes have now been committed. If so, the transaction must be aborted.
	* Detecting writes that affect prior reads
		* read is recorded and notified if there's a write later.

# Concurrent issues
## Dirty Write
## Lost Updates
### example
* concurrent counter increment example
### how to prevent
* automatically detecting lost updates
* atomic write operations
* explicit locking
* compare and set
* conflict resolution and replication
	* locks and compare and set assume there's a single up to date copy of the data
	* atomic operations work well in a replicated context, especially if they are commutative


## Write Skew & Phantoms
### example
* oncall doctors
* meeting room booking
### occurance patterns
* issue a query
* make a decision based on the result from the last step
* write result to database which will change the result of the first query
* order of steps can be changed
### how to prevent
* true serilization
* lock
	* materializing conflicts
