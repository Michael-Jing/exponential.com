+++
title = "Java Memory Model"
date = 2020-12-29
+++

## How do final fields work under the new JMM?

Assuming the object is constructed "correctly", once an object is constructed, the
values assigned to the final fields in the constructor will be visible to all
other threads without synchronization. In addition, the visible values for any
other object or array referenced by those final fields will be at least as up-to-date
as the final fields.

what does it mean for an object to be properly constructed? It simply means that
no reference to the object being constructed is allowed to "escape" during construction.

You can have a final pointer to an array and not have to worry about other therads
seeing the correct values for the array reference, but incorrect values for the
contents of the array. Again, by "correct" here, we mean "up to date as of the end
of the object's constructor", not "the latest value available".

## What does volatile do?

Under the new memory model, it is still true that volatile variables cannot be reordered
with each other. In addition, it is now no longer so easy to reorder normal field
accesses around them. Writing to a volatile field has the same memory effect
as a monitor release, and reading from a volatile field has the same memory
effect as a monitor acquire. In effect, because the new memory model places
stricter constraints on reordering of volatile field accesses, volatile or not,
anything that was visible to thread A when it writes to volatile field f becomes
visible to thread B when it reads f.
```java
class VolatileExample {
    int x = 0;
    volatile boolean v = false;
    public void writer() {
        x = 42;
        v = true;
    }
    public void reader() {
        if (v == true) {
            // uses x - guaranteed to see 42, even though x is not volatile
            // because anything that was visible to thread A when it writes
            // to volatile field v becomes visible to thread B when it reads v
        }
    }
}

```
<strong>Important Note:</strong> Note that it is important for both threads to access
the same volatile variable in order to properly set up the happens-before relationship. It is not the case that everything visible to thread A when it writes to volatile
field f becomes visible to thread B after it reads volatile field g.
