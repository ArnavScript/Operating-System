# Reader-Writer Problem in Operating Systems

The **Reader-Writer Problem** is a classic synchronization problem in operating systems. It deals with a shared resource (like a file or database) that can be read by multiple processes simultaneously, but must be written by only one process at a time. The challenge is to allow concurrent reads while ensuring exclusive access for writers.

## Problem Statement

- **Readers**: Multiple readers can read the resource at the same time.
- **Writers**: Only one writer can write at a time, and no readers can read during writing.

## Types of Reader-Writer Problems

1. **First Readers-Writers Problem**: No reader should be kept waiting unless a writer has already obtained permission to use the resource.
2. **Second Readers-Writers Problem**: Once a writer is ready, it should be given preference over readers.

## Semaphore Solution

Semaphores are used to synchronize access to the shared resource.

### Variables

- `mutex`: Semaphore to protect the `read_count` variable.
- `rw_mutex`: Semaphore to control access to the resource.
- `read_count`: Number of readers currently accessing the resource.

### Pseudocode

#### Reader Process

```c
wait(mutex);
read_count++;
if (read_count == 1)
    wait(rw_mutex); // First reader locks the resource
signal(mutex);

// Reading is performed

wait(mutex);
read_count--;
if (read_count == 0)
    signal(rw_mutex); // Last reader unlocks the resource
signal(mutex);
```

#### Writer Process

```c
wait(rw_mutex);

// Writing is performed

signal(rw_mutex);
```

### Explanation

- **Readers**: The first reader locks the resource for reading. Multiple readers can enter as long as no writer is active. The last reader unlocks the resource.
- **Writers**: Writers wait until no readers or other writers are accessing the resource.

## Key Points

- Prevents race conditions.
- Ensures data consistency.
- Can be modified for reader or writer preference.

