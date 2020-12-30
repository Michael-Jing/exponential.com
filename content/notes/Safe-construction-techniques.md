+++
title = "Safe construction techniques"
date = 2020-12-29
+++

## Don't publish the "this" reference during construction
```java
public class EventListener {
    public EventListener(EventSource eventSource) {
        // do our initialization
        ...
        // register ourselves with the event source
        eventSource.registerListener(this);
    }
}
```
Even ignoring all the Java memory Model issues such as differences in visibility
accross threads and memory access reording, this code still is in danger of
exposing an incompletely constructed EventListener object to other threads.
Consider what happens when EventListener is subclassed, as below:
```java
public class RecordingEventListener extends EventListener {
    private final ArrayList list;
    public RecordingEventListener(EventSource eventSource) {
        super(eventSource);
        list = Collections.synchronizedList(new ArrayList());
    }
    public onEvent(Event e) {
        list.add(e);
        super.onEvent(e);
    }
}
```
If the event source decides to send an event from within the registration call,
or we just get unlucky and an event arrives at exactly the wrong moment,
```RecordingEventListener.onEvent()``` could get called while list still has
the default value of null.

## Don't implicitly expose the "this" reference
```java
public class EventListener2 {
    public EventListener2(EventSource eventSource) {
        eventSource.registerListener(
          new EventListener() {
            public void onEvent(Event e) {
                eventReceived(e);
              }
          }
        )
      }
      public void eventReceived(Event e) {}
  }
  ```
Non-static inner classes maintain an implicit copy of the ```this``` reference of
their parent object, so creating an anonymous inner class instance and passing it
to an object visible from outside the current thread has all the same risks as
exposing the ```this``` reference itself.

## Don't start threads from within constructors
Actually I'm get confusing on this, my question is that is what if we want to have
an instance field that points to a class that start threads within constructors?
or if the class provides a ```start()``` method, is it ok for us to call the
```start()``` method in our constructor. if the newly started threads need to
access our object, then i think it not ok, since our object is not fully constructed
yet.

## What does "publish" mean?
```java
public class Safe {

  private Object me;
  private Set set = new HashSet();
  private Thread thread;

  public Safe() {
    // Safe because "me" is not visible from any other thread
    me = this;

    // Safe because "set" is not visible from any other thread
    set.add(this);

    // Safe because MyThread won't start until construction is complete
    // and the constructor doesn't publish the reference
    thread = new MyThread(this);
  }

  public void start() {
    thread.start();
  }

  private class MyThread(Object o) {
    private Object theObject;

    public MyThread(Object o) {
      this.theObject = o;
    }

    ...
  }
}

public class Unsafe {
  public static Unsafe anInstance;
  public static Set set = new HashSet();
  private Set mySet = new HashSet();

  public Unsafe() {
    // Unsafe because anInstance is globally visible
    anInstance = this;

    // Unsafe because SomeOtherClass.anInstance is globally visible
    SomeOtherClass.anInstance = this;

    // Unsafe because SomeOtherClass might save the "this" reference
    // where another thread could see it
    SomeOtherClass.registerObject(this);

    // Unsafe because set is globally visible
    set.add(this);

    // Unsafe because we are publishing a reference to mySet
    mySet.add(this);
    SomeOtherClass.someMethod(mySet);

    // Unsafe because the "this" object will be visible from the new
    // thread before the constructor completes
    thread = new MyThread(this);
    thread.start();
  }

  public Unsafe(Collection c) {
    // Unsafe because "c" may be visible from other threads
    c.add(this);
  }
}
```

## Conclusion
Try to avoid using this, Try to avoid creating instances of inner classes or
starting threads from constructors. If you cannot avoid using this, be very
sure that you are not making the ```this``` reference visible to other threads.
