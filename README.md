safe-thread-local-random
========================
An implementation of [Random](https://docs.oracle.com/javase/8/docs/api/java/util/Random.html)
that delegates all methods to
[ThreadLocalRandom.current()](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ThreadLocalRandom.html#current--).

Usage
-----

Simple example:
```java
Random random = SafeThreadLocalRandom.get();
int random = random.nextInt();
```

Given the following `IdGenerator.java`:

```java
/** Simple ID generator (not cryptographically secure). */
@ThreadSafe
public final class IdGenerator {
  private final Random random;
  public IdGenerator(Random random) {
    this.random = random;
  }

  public long getId() {
    return random.nextLong();
  }
}
```

We would _never_ want to pass the result of `ThreadLocalRandom.current()` directly, because we would associate the
random instance from our current thread with a utility that is shared between other threads, resulting in degraded
performance. Passing the result of `SafeThreadLocalRandom.get()` produces an improvement because there is no
contention between threads[1], all invocations are passed through to the calling thread's `ThreadLocalRandom`.

1. From https://docs.oracle.com/javase/8/docs/api/java/util/Random.html:
   > Instances of java.util.Random are threadsafe. However, the concurrent use of the same java.util.Random instance
   > across threads may encounter contention and consequent poor performance. Consider instead using ThreadLocalRandom
   > in multithreaded designs.

Gradle Tasks
------------
`./gradlew tasks` - to get the list of gradle tasks


Start Developing
----------------
Run one of the following commands:

* `./gradlew idea` for IntelliJ
* `./gradlew eclipse` for Eclipse
test
