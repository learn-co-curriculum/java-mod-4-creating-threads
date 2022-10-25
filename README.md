# Creating Your Own Threads

## Learning Goals

- Create custom threads in Java.
- Create simple multithreaded programs.

## Creating Custom Threads in Java

We can create and run custom threads from the main thread.
Java gives us two primary ways to create custom threads:

- Instantiate a class that extends `Thread` and overrides the `run()` method.
- Pass into the `Thread` constructor an object that implements `Runnable`, which is either:
  - an instance of a class that implements `Runnable`
  - a lambda expression that implements `Runnable`

### A class that extends `Thread`

In this case, we define a new class `PrintingThread` that
extends the `Thread` class and overrides the `run()` method.
We can create threads as instances of the extended class
and call `start()` on an instance, which causes the `run()`
method to eventually be invoked.  Threads have several properties
including a name and id.  The
`main` method below prints the name and id of each thread, while
the `run` method prints the current thread's name and the loop counter value:

```java
public class PrintingThread extends Thread {
    @Override
    public void run() {
        for (int i=1; i<=5; i++) {
            System.out.println(Thread.currentThread().getName() + " i=" + i);
        }
    }

    public static void main(String[] args) {
        Thread main = Thread.currentThread();   //main thread
        System.out.println("Thread id " + main.getId() + " name " + main.getName());
        PrintingThread t0 = new PrintingThread();
        PrintingThread t1 = new PrintingThread();
        System.out.println("Thread id " + t0.getId() + " name " + t0.getName());
        System.out.println("Thread id " + t1.getId() + " name " + t1.getName());
        t0.start();  
        t1.start();  
    }
}
```

The output shows the data from the default `Thread` instance named `main`, along with the two
`PrintingThread` objects that take turns executing their `run()` methods.

```text
Thread id 1 name main
Thread id 12 name Thread-0
Thread id 13 name Thread-1
Thread-1 i=1
Thread-1 i=2
Thread-0 i=1
Thread-0 i=2
Thread-0 i=3
Thread-1 i=3
Thread-1 i=4
Thread-0 i=4
Thread-0 i=5
Thread-1 i=5
```

A program should not explicitly call the `run()` method.
Instead, we call the `start()` method to request that
the thread begin its execution.  The JVM will
automatically call the `run()` method after the thread starts,
although it may not happen immediately.

If you start multiple threads, the order of execution isn’t guaranteed either.
Sometime `Thread-0` prints before `Thread-1`, other times `Thread-1` prints
before `Thread-0`. However, the statements inside a single
thread do execute sequentially.


### A class that implements `Runnable` 

In this case, we define a new class `PrintingRunnable`
that implements the `Runnable` interface by overriding the `run()` method.
To create a thread, we pass an instance of `PrintingRunnable` to the `Thread` constructor.

```java
public class PrintingRunnable implements Runnable {
    @Override
    public void run() {
        for (int i=1; i<=5; i++) {
            System.out.println(Thread.currentThread().getName() + " i=" + i);
        }
    }

    public static void main(String[] args) {
        Thread main = Thread.currentThread();   //main thread
        System.out.println("Thread id " + main.getId() + " name " + main.getName());
        Thread t0 = new Thread(new PrintingRunnable());
        Thread t1 = new Thread(new PrintingRunnable());
        System.out.println("Thread id " + t0.getId() + " name " + t0.getName());
        System.out.println("Thread id " + t1.getId() + " name " + t1.getName());
        t0.start();  
        t1.start();  
    }
}
```

### A lambda expression that implements `Runnable`

The `Runnable` interface is a functional interface which means we can use lambda
expressions for our `run()` method implementation.   We've put the loop in a
static method to avoid code duplication between the two thread instantiations.
We could also use the method reference `PrintingLambda::PrintTask` in place
of the lambda expression:

```java
public class PrintingLambda  {

  public static void PrintTask() {
    for (int i=1; i<=5; i++) {
      System.out.println(Thread.currentThread().getName() + " i=" + i);
    }
  }

  public static void main(String[] args) {
    Thread main = Thread.currentThread();   //main thread
    System.out.println("Thread id " + main.getId() + " name " + main.getName());
    Thread t0 = new Thread( () -> PrintTask() );
    Thread t1 = new Thread( () -> PrintTask() );
    System.out.println("Thread id " + t0.getId() + " name " + t0.getName());
    System.out.println("Thread id " + t1.getId() + " name " + t1.getName());
    t0.start();  
    t1.start();  
  }
}
```


## Conclusion

We’ve learned how to create and start our own custom threads. In the next
lessons, we’ll learn more about how to manage the threads and also look at how
to safely manage shared data among threads.


## Resources

[Java 11 Thread](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Thread.html)  
[Java 11 Runnable](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Runnable.html)  

