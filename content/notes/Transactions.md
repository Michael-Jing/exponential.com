+++
title=  "Transactions"
date=   2021-06-12 20:49
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

## Preventing Lost Updates
