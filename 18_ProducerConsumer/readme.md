# Producer-Consumer Problem Using Semaphores

## Overview

The Producer-Consumer problem is a classic synchronization problem. It involves two processes: the **Producer**, which generates data and puts it into a buffer, and the **Consumer**, which takes data from the buffer. The challenge is to ensure that the producer does not add data into a full buffer and the consumer does not remove data from an empty buffer.

## Key Concepts

- **Buffer**: Shared memory area for data exchange.
- **Semaphore**: Synchronization tool to control access to shared resources.
    - **Mutex**: Ensures mutual exclusion for buffer access.
    - **Empty**: Counts empty slots in the buffer.
    - **Full**: Counts filled slots in the buffer.

## Semaphores Used

- `mutex` (initial value = 1): Ensures only one process accesses the buffer at a time.
- `empty` (initial value = buffer size): Tracks available empty slots.
- `full` (initial value = 0): Tracks filled slots.

## Solution (Pseudocode)

### Producer

```c
do {
        // Produce an item
        wait(empty);    // Decrement empty count
        wait(mutex);    // Enter critical section

        // Add item to buffer

        signal(mutex);  // Exit critical section
        signal(full);   // Increment full count
} while (true);
```

### Consumer

```c
do {
        wait(full);     // Decrement full count
        wait(mutex);    // Enter critical section

        // Remove item from buffer

        signal(mutex);  // Exit critical section
        signal(empty);  // Increment empty count
} while (true);
```

## Detailed Explanation

1. **Producer**:
     - Waits for an empty slot (`wait(empty)`).
     - Acquires mutex to enter the critical section (`wait(mutex)`).
     - Places the produced item in the buffer.
     - Releases mutex (`signal(mutex)`).
     - Signals that a new item is available (`signal(full)`).

2. **Consumer**:
     - Waits for a filled slot (`wait(full)`).
     - Acquires mutex to enter the critical section (`wait(mutex)`).
     - Removes an item from the buffer.
     - Releases mutex (`signal(mutex)`).
     - Signals that a slot is now empty (`signal(empty)`).

## Why Semaphores?

Semaphores prevent race conditions and ensure that:
- The producer waits if the buffer is full.
- The consumer waits if the buffer is empty.
- Only one process accesses the buffer at a time.

## Diagram

```
+----------+      +--------+      +----------+
| Producer | ---> | Buffer | ---> | Consumer |
+----------+      +--------+      +----------+
            ^               ^                ^
            |               |                |
     empty/full       mutex           empty/full
    (semaphores)   (semaphore)      (semaphores)
```

## Conclusion

Using semaphores, the Producer-Consumer problem is solved efficiently, ensuring synchronization, mutual exclusion, and avoiding deadlocks or race conditions.