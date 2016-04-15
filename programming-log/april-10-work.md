#Java homework 9
## The Order of fields in a Class

When we build a class, we always have the following order, static variables and methods, then non-static variables and methods, the reason is:
every field could only refer things above, for example:

```java
public class Factory {

    private static final Semaphore workSemaphore = new Semaphore(1);

    public static Producer createProducer() {
 	  // TODO
        return new VendingMachineProducer(workSemaphore);
    }

}
```
the variable `workSemaphore` must be static. see the next example won't work:

```java
public class Factory {
    // ERROR EXAMPLE
    private final Semaphore workSemaphore;

    public Factory(){
      workSemaphore = new Semaphore(1);
    }

    public static Producer createProducer() {
 	  // TODO
        return new VendingMachineProducer(workSemaphore);
    }

}
```

The reason is easy in Java standard, but **for convenience and memory purpose**, always write static field at first, and static field only can refer to static field.
