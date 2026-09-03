# Process Synchronization, Deadlocks, and Memory Management

## Race Condition

When more than one process is executing the same code, or accessing the same memory or any shared variable, there's a possibility that the output or the value of the shared variable ends up wrong, because all the processes are effectively racing to have their output be the one that's correct. This condition is known as a race condition.

## Critical Section

A critical section is a code segment that accesses shared variables and has to be executed as an atomic action. Only one process must be executing its critical section at a given time.

```
do {
    entry section
    critical section
    exit section
    remainder section
} while (true)
```

### Critical Section Solution requirements

- **Mutual exclusion**: Only one process should execute in its critical section at a time.
- **Progress**: If no process is executing in its critical section and some process wishes to enter, only processes not executing in their remainder section can participate in deciding who goes next.
- **Bounded waiting**: There's a limit on how many times other processes are allowed to enter their critical section before a waiting process gets its turn.

## Producer-Consumer Problem

### Producer

```
do {
    wait (empty);
    wait (mutex);
    // produce item
    signal (mutex);
    signal (full);
} while (TRUE);
```

### Consumer

```
do {
    wait (full);
    wait (mutex);
    // consume item
    signal (mutex);
    signal (empty);
} while (TRUE);
```

## Readers-Writers Problem

Modeled on access to a database:

- A **reader** is a thread that needs to look at the database but won't change it.
- A **writer** is a thread that modifies the database.

Example: making an airline reservation. Browsing flight schedules acts as a reader on your behalf; reserving a seat requires writing into the database.

**Priority rules:**
1. No reader will be kept waiting unless a writer has already obtained permission to use the shared object.
2. If a writer is ready, it waits for the minimum amount of time.

**Shared data:**
- Data set
- Semaphore `mutex`, initialized to 1
- Semaphore `wrt`, initialized to 1
- Integer `readcount`, initialized to 0

### Writer process

```
do {
    wait (wrt);
    // writing is performed
    signal (wrt);
} while (TRUE);
```

### Reader process

```
do {
    wait (mutex);
    readcount++;
    if (readcount == 1)
        wait (wrt);
    signal (mutex);

    // reading is performed

    wait (mutex);
    readcount--;
    if (readcount == 0)
        signal (wrt);
    signal (mutex);
} while (TRUE);
```

## Dining Philosophers Problem

Five philosophers who spend their lives just thinking and eating, with only five chopsticks available between them. Each philosopher thinks; when hungry, sits down, picks up the two chopsticks closest to them, and eats. After eating, puts the chopsticks down and goes back to thinking.

**Shared data:**
- Bowl of rice (data set)
- Semaphore `chopstick[5]`, each initialized to 1

**Possibility of deadlock**: if all philosophers become hungry at the same time and each picks up their left chopstick, a deadlock occurs (every philosopher holds one chopstick and waits forever for the second).

**Possible solutions:**
- Allow at most four philosophers to sit at the table simultaneously.
- Allow a philosopher to pick up chopsticks only if both are available (done inside a critical section).
- Use an asymmetric solution: odd-numbered philosophers pick up their left chopstick first, even-numbered philosophers pick up their right chopstick first.

### Structure of Philosopher i

```
do {
    wait (chopstick[i]);
    wait (chopstick[(i + 1) % 5]);

    // eat

    signal (chopstick[i]);
    signal (chopstick[(i + 1) % 5]);

    // think
} while (TRUE);
```

## Deadlock

A set of blocked processes, each holding a resource and waiting to acquire a resource held by another process in the set.

**Example**: system has 2 disk drives; P1 and P2 each hold one disk drive and each needs the other one.

**Example**: semaphores A and B, both initialized to 1:

```
P0              P1
wait(A);        wait(B);
wait(B);        wait(A);
```

If both processes acquire their first semaphore before either can acquire the second, neither can proceed.

### Deadlock Characterization

Deadlock can arise if four conditions hold simultaneously:

- **Mutual exclusion**: Only one process at a time can use a resource.
- **Hold and wait**: A process holding at least one resource is waiting to acquire additional resources held by other processes.
- **No preemption**: A resource can be released only voluntarily by the process holding it, after that process completes its task.
- **Circular wait**: There exists a set of waiting processes {P0, P1, ..., Pn} such that P0 is waiting for a resource held by P1, P1 is waiting for a resource held by P2, and so on, with Pn waiting for a resource held by P0.

### Resource-Allocation Graph

- A **claim edge** (Pi -> Rj), shown as a dashed line, indicates that process Pi may request resource Rj in the future.
- A claim edge converts to a **request edge** when the process actually requests the resource.
- A request edge converts to an **assignment edge** once the resource is allocated to the process.
- When the resource is released, the assignment edge reconverts to a claim edge.
- Resources must be claimed a priori (declared in advance) in the system.

### Deadlock Avoidance

Requires the system to have additional a priori information about how processes use resources.

- Simplest and most useful model: each process declares the maximum number of resources of each type it may need.
- The deadlock-avoidance algorithm dynamically examines the resource-allocation state to ensure a circular-wait condition can never occur.
- The resource-allocation state is defined by the number of available and allocated resources, plus the maximum demands of the processes.

## Paging

A storage mechanism used to retrieve processes from secondary storage into main memory, in the form of pages.

- Each process is divided into pages; main memory is divided into frames of the same size.
- Page size must equal frame size.
- Pages of a process are brought into main memory only when required; otherwise they remain in secondary storage.

## Segmentation

- Process isn't divided blindly into fixed-size pages; instead it's divided into modules, for better visualization.
- Segmentation is a variable-size partitioning scheme.
- Secondary memory and main memory are divided into partitions of unequal size, depending on the length of each module.
- The partitions of secondary memory are called segments.

## Demand Paging

When a process is swapped in, its pages aren't all swapped in at once. Instead, they're swapped in only when the process actually needs them (on demand).

## Page Fault

If a referenced page isn't present in main memory, that's a miss, called a page fault. The CPU has to fetch the missed page from secondary memory. If the number of page faults is very high, the effective access time of the system becomes very high as well.

## Thrashing

If the number of page faults approaches the number of referenced pages, or is otherwise very high, the CPU spends most of its time just reading pages back in from secondary memory. Effective access time becomes roughly the time it takes to read one word from secondary memory, which is very slow. This condition is called thrashing.

## Cryptography

Cryptography is the practice of securing information by transforming it into a format that's unreadable to unauthorized users. It involves encryption (converting plain text into ciphertext) and decryption (converting ciphertext back into plain text). Cryptography ensures the confidentiality, integrity, and authenticity of data, making it a crucial part of modern cybersecurity.
