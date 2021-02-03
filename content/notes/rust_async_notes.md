+++
title = "Rust Async Notes"
date = 2021-01-12

+++
<strong>Due to the nature of notes, the content of this notes would be ustructured</strong>

## .await ing on a Multithreaded Executor <br>
  ```Future``` may move between threads, so any variables used in ```async```
  bodies must be able to travel between threads. So it is not safe to use ```Rc```, ```RefCell``` or any other types that don't implement the ```Send```
  trait, (it is possible to use these types so long as they aren't in scope
  during a call to ```.await```.). Similarly, it isn't a good idea to hold a
  traditional non-futures-aware lock across an ```.await```, as it can cause
  the threadpool to lock up: one task could take out a lock, ```.await``` and yield to the executor, allowing another task to attempt to take the lock and
  cause a deadlock. To avoid this, use the ```Mutex``` in ```futures::lock```

## Pin
1. For pinned data, you have to maitain the ```invariant``` that its memory will not get
invalidated or repurposed from the moment it gets pinned until when drop is called.
so the question here is if I moved the pinned data, what can I expect? a compile error or a
runtime error or undefined behavior? how can I do an experiment on this

2. Pinning a ```!Unpin``` object to the stack requires ```unsafe```, why?

3. Getting a ```&mut T``` to a pinned T requires unsafe if ```T: !Unpin```.

4. If ```T: Unpin``` (which is the default), then ```Pin<'a, T>``` is entirely equivalent to
```&'a mut T```. In other words: ```Unpin``` means it's OK for this type to be moved even when
pinned, so ```Pin``` will have no effect on such a type .
