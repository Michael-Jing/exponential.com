+++
title = "ReentrantLock in Java"
date = 2020-12-30
+++

## The advantage if ReentrantLock over intrinsic Lock
* The thread waiting to acquire a lock can be interrupted if ```lockInterruptibly()``` is used
* A timeout can be set by using ```tryLock()```
* Hand-over-Hand Locking is possible which enables efficient concurrent access on data structures like linked list
* Conditional variables is introduced
 pattern to use conditional variables
 ```java
 ReentrantLock lock = new ReentrantLock();
 Condition condition = lock.newCondition();
 lock.lock();
 try {
    while (!<<condition is true>>)
        condition.await();
    « use shared resource>>
 } finally {lock.unlock(); }
 ```
A thread must hold the associated lock before being able to wait on the condition. if the condition is not true, it calls ```await()```
, which atomically unlocks the lock and blocks on the condition variable.

When another thread calls ```singal()``` or ```singnalAll()``` to indicate that
the condition might now be true, ```await()``` unblocks and automatically
reacquires the lock. An important point is that when ```await()``` returns,
it only indicates that the condition might be true. so we need to go back and
recheck whether the condition is true.

From my understanding, the advantage of condition is that we can use ```signal()``` and ```await()``` to get some coordination between threads, instead of trying
to acquire the lock blindly.

How can we solve the Dining Philosoper problem using condition? In order to eat,
a philosoper need to have his left chopstick and his right chopstick available,
so intuitively, we might want to associate a condition with each of the chopstick,
and a philosoper can wait on the condition associated with the chopsitck on
his left and the chopsitck on his right in order to eat. But, the issue is,
waiting on two conditions is messy. So, instead, we associate a condition
with each of the philosoper, and the condition only means the philosoper may
go ahead to try to eat, but it's not guaranteed that both of the chopsticks
are available. When a philosoper finishes eating, he needs to singal both the
philosoper on his left and the philosoper on his right. In order to eat, a
philosoper needs to make sure that both pholosopers on his left and his right
is not eating.

## Fairness
If fairness parameter is set to true, under contention, locks favor granting access to the
longest-waiting thread. Otherwise this lock does not guarantee any particular acces order.
Programs using fair locks accessed by many threads may display lower overall throughput (i.e., are slower; often much slower) than those using the default setting. Note however, that fairness
of locks does not guarantee fairness of thread scheduling. Thus, one of many threads using
a fair lock may obtain it multiple times in succession while other active threads are not
progressing and not currently holding the lock. Also note that the untimed ```tryLock``` method
does not honor the fairness setting.

## Spurious wakeup
A <strong>spurious wakeup</strong> happens when a thread wakes up from waiting on a condition
variable that's been signaled, only to discover that the condition it was waiting for isn't
satisfied.

Because spurious wakeups can happen whenever there's a race and possibly even in the absence of
a race or a signal, when a thread wakes on a condition variable, it should always check that
the condition it sought is satisfied.
