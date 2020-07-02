---
layout: post
title:  "SSTables and LSM-Trees"
date:   2020-07-02 17:54:00+0800
categories: jekyll update
---

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



