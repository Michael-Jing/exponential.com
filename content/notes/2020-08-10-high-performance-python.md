+++
title=  "High Performance Python"
date=   2020-08-10 14:53:00
+++

# High Performance Python

## Benchmarking & Profiling

### Test & Benchmarking

- tiem command
- timeit module in Python
- pytest-benchmark
- timeit as a decorator
- timeit as a context manager

### Find Bottlenecks
- cProfile
- line_profiler
- memory_profiler

## Threads Synchronization

### Synchronization primitives

- join
- Lock
- RLcok
  - 谁拿谁放
  - 同一线程多次拿到
  - 拿多少次放多少次
- condition
- semaphores
- Events
- barrier

### Concurrent DS
#### Thread-safe in Python
- queue
  - Queue
  - LifoQueue
  - PriorityQueue
- Deque
#### Not Thread-safe in Python
- Set
- List
#### Make Non Thread-safe Thread-safe
- Extend
- decorator for method
- decorator for class

## Executors & Pools
### Based on
- Threads
- Process
### Apis
- for test
  - set_exception
  - set_result
  - set_running_notify_cancel
- shutdown
  - executor.shutdown
- future objects apis
  - query
    - running
    - done
    - result
    - exception
    - as_completed
  - control
    - add_done_callback
    - cancel

## Processes
### Create
- multiprocessing.Process
- Inherit class Process
### Apis
- name
- daemon
- terminate
### Synchronization
- Lock
- Rlock
- Condition
- Semaphore
- Event
- Barrier
### Communicate
- Queue
- Pipe
  - bidirectional
    - multiprocessing.Pipe
  - unidirectional
    - os.pipe
- Manager
- multiprocessing.connection
### Pool
- apply & apply_async
- map & map_async
- starmap & starmap_async
- Imap & Imap_unordered
- maxtaskperchild
