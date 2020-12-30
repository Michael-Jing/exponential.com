+++
title = "Atomic field updaters"
date = 2020-12-30
+++

<strong>Field updaters</strong> are essentially used as "wrappers" around a volatile field
(primitive or object reference). They are generally used when one or both of the following are true:
* You generally want to refer to the variable "normally" (that is, without having to always refer to it via the get or set methods on the atomic classes) but occasionally need an atomic  get-set operation (which you can't do with a normal volatile field);
* You are going to create a large number of objects of the given type, and don't want every single instance to have to have an extra object embeded in it just for atomic access.

An example of the first kind is BufferedInputStream. Every instance of BufferedInputStream
contains an internal buffer, defined as a volatile byte array. Generally speaking, BufferedInputStream just
needs to refer this array "normally" for the purpose of reading/writing bytes. However, an atomic
get-and-set operation is needed in specific places where the buffer array is replaced (either to
grow it or to mark it as null or closed), because closure can occur in a different thread to that
performing the reads. One option would have been to declare the buffer variable as a AtomicReference to an array, and always get and set the array via this variable. But that would be a bit
messy. Instead, a normal volatile array variable is used, and the class also contains a static
AtomicReferenceUpdater as follows:
```java
static AtomicReferenceFieldUpdater<BufferedInputStream, bytes[]>
    bufUpdater = AtomicReferenceFieldUpdater.newUpdater(BufferedInputStream.class,
    byte[].class, "buf");
```

The array itself still remains a normal (volatile) variable, and reads and writes from the array
can use normal array syntax. But when necessary, via the field updater, an atomic comparae-and-set
operation can be performed on the volatile variable. Although the update is static, each instance
of BufferedInputStream has its own buffer of course. So when we perform the CAS operation, we
need to pass into the updater the actual object whose variable we want to affect:
```java
if (bufUpdater.compareAndSet(this, oldbuffer, newBuffer)) {
  ...
}
```
