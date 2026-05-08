---
title: "Concurrency"
date: 2021-12-28
---

# Blocking

- Blocking
- Starvation Free

# Obstruction free

# Lock free(LF)

![](_20211228_132424screenshot.png) at least some thread is doing progress on its work

# Wait free(WF)

any given thread provided with a time-slice will be able to make some progress and eventually complete.

## Wait-Free Bounded(WFB)

```cpp
AtomicIntegerArray intArray = new AtomicIntegerArray(MAX_THREADS);

public void funcWaitFreeBounded() {
          for (int i = 0; i < MAX_THREADS ; i++) {
                  intArray.set(i, 1);
          }
}

```

## Wait-Free Population Oblivious(WFPO)

```cpp
atomic<int> counter;
void funcWaitFreeBoundedPopulationOblivious() {
    counter.fetch_add(1);
}
```

# hazzard pointer

- protect
- retain
- exchange

# lock

锁要保证公平性
