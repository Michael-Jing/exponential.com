+++
title = "Correct Way to Do Double Locking"
date = 2020-12-29
+++

```java
class Foo {
    private volatile Helper helper = null;
    public Helper getHelper() {
        Helper result = helper;
        if (result == null) {
            synchronized(this) {
                result = helper;
                if (result == null) {
                    helper = result = new Helper();
                }
            }
        }
        return result;
    }

    // other functions and members...
}
```
Another way that works
```java
public class FinalWrapper<T> {
    public final T value;
    public FinalWrapper(T value) {
        this.value = value;
    }
}

public class Foo {
   private FinalWrapper<Helper> helperWrapper = null;

   public Helper getHelper() {
      FinalWrapper<Helper> wrapper = helperWrapper;

      if (wrapper == null) {
          synchronized(this) {
              if (helperWrapper == null) {
                  helperWrapper = new FinalWrapper<Helper>(new Helper());
              }
              wrapper = helperWrapper;
          }
      }
      return wrapper.value;
   }
}
```
if the wanted object is static, then lazy initialization pattern could be used
```java
@ThreadSafe
class Foo {
    private static class HelperHolder {
       public static Helper helper = new Helper();
    }

    public static Helper getHelper() {
        return HelperHolder.helper;
    }
}
```
