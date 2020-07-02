---
layout: post
title:  "SSTables and LSM-Trees"
date:   2020-07-02 17:54:00+0800
categories: jekyll update
---
# SSTables and LSM-Trees
SStable stands for Sorted String Table, in which key sequence of key-value pairs is sorted by key. 
* A compaction process is running in the background to merge SSTables and remove old values of keys. mergesort is used.
* In memory index is sparse, keys between two consecutive indexed keys need to be scanned.
* Since read requests need to scan over several key-value pairs in the requested range anyway, it is possible to group those records 
into a block and compress it before writing it to disk.

## Constructing and maintaining SSTables
1.  When a write comes in, add it to an in-memory balanced tree data structure. This in-memory tree is sometimes called a memtable.
2. When the memtable gets bigger than some threshold, write it out to disk as an SSTable file.
3. In order to server a read request, first try to find the key in the memtable, then in on-disk segment
4. run a compaction process to combine segment files and discard overwritten or deleted values.
5. use a append log to recover from crash. 
## Making an LSM-tree out of SSTables
* use Bloom filter to efficiently detect keys that does not exist
* size-tiered and leveled compaction
    * size-tiered: newer and smaller SSTables are successively merged into older and larger SSTables.
    * leveled compaction: the key range is split up into smaller SSTables and older data is moved into separate 'levels'
# B-Trees
## Making B-trees reliable
* write-ahead log (WAL)
* protecting the tree's data structures with latches (lightweight locks)
## B-tree optimizations
* copy-on-write. A modified page is witten to a different location, and a new version of the parent pages is created, pointing at the new location.
* save space by abbreviating keys
* additional pointers
* borrow log-structured ideas
# Comparing B-Trees and LSM-Trees
* LSM-trees are typically faster for writes, whereas B-trees are thought to be faster for reads. Reads are typically slower on LSM-trees because they have to check several different data structures and SSTables at different stages of compaction.

* Need to test systems with particular workload in order to make a valid comparison. 

## Advantages of LSM-trees
write amplification: one write to the database resulting in multiple writes to the disk over the course of the database's lifetime.
* In write-heavy applications, the performance bottleneck might be the rate at which the database can write to disk. The more that a storage engine writes to disk, the fewer writes per second it can handle within the available disk bandwidth.

* LSM-trees are typically able to sustain higher write throughput than B-trees, partly because they sometimes have lower write amplification (depends on the storage engine configuration and workload), and partly because they sequentially write SSTable files
* LSM-trees can be compressed better
* SSD firmware may use a log-structured algorithm to turn random writes into sequential wirtes

## Downsides of LSM-trees
* The compaction process can sometimes interfere with the performance of ongoing reads and writes. the response time of queries to log-structured storage engines can sometimes be quite high
* At high write throughput, the compaction may cannot keep up with the rate of incoming writes.
* An advantage of B-trees is that each key exists in exactly one place, so transaction isolation is easier.



