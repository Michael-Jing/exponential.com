+++
title = "Tips on  Using Locks"
date = 2020-12-29 10:00:00
+++
The content of this note is from the book "Seven Concurrency Models in Seven Weeks"
## Do Not Use an Object's Hash to Order Locks

One piece of advice you'll often see is to use an object's hash code to order lock
acquisition, such as shown here:

```java
if (System.identityHashCode(left) < System.identityHashcode(right)) {
    first = left; second = right;
} else {
    first = right; second = left;
}
```

The issue is that when the two objects left and right have the same hascode, then the
result of the ordering will be nonderministic.

## Do Not Use Alien Methods While Honlding A Lock

Large programs often make use of listeners to decouple modules. Here, for example, is a
class that downloads from a URL and allows ProgressListeners to be registered:
```java
class Downloader extends Thread {
    private InputStream in;
    private OutputStream out;
    private ArrayList<ProgressListener> listeners;

    public Downloader(URL url, String outputFilename) throws IOException {
        in = url.openConnection().getInputStream();
        out = new FileOutputStream(outputFilename);
        listeners = new ArrayList<ProgressListener>();
    }
    public synchronized void addListener(ProgressListener listener) {
        listeners.add(listener);
    }
    public synchronized void removeListener(ProgressListener listener) {
        listeners.remove(listener);
    }
    private synchronized void updateProgress(int n) {
        for (ProgressListener listener: listeners)
            listener.onProgress(n);
    }
}

```
Because addListener(), removeListener(), and updateProgress() are all synchronized, multiple
threads can can them without stepping on one another's toes. But a trap lurks in this code
that could leak to deadlock enev though there's only a single lock in use.

The problem is that updateProgress() calls an alien method - a method it knows nothing about.
That method could do anything, including acquiring another lock. If it does, then we've
acquired two locks without knowing whether we've done so in the right order. That can lead to
deadlock.

The only solution is to avoid calling alien methods while holding a lock. One way to achieve
this is to make a defensive copy of listeners before iterating through it:

```java
private void updateProgress(int n) {
    ArrayList<ProgressListener> listenersCopy;
    synchronized(this) {
        listenersCopy = listeners.clone();
    }
    for (ProgressListener listener: listenersCopy)
        listener.onProgress(n);
}
```

This change kills several birds with one stone. Not only does it avoid calling an alien
method with a lock held, it also minimizes the period during which we hold the lock. Holding
locks for longer than necessary both hurts performance (by restricting the degree of achievable concurrency) and increases the dange of deadlock. This change also fixes another buy that
isn't related to concurrency - a listener can now call removeListener() within its onProgress() method without modifying the copy of listeners that's mid-iteration.

## Takeaway tips

* Synchronize all access to shared variables.
* Both the writing and the reading threads need to use synchronization
* Acquire multile locks in a fixed, global order, and don't use hascode to determine the global order
* Don't call alien methods while holding a lock.
* Hold locks for the shortest possible amount time.
