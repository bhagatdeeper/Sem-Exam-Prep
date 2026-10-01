# Operating Systems — Semester Exam Study Guide

*A compact, beginner-friendly guide for a one-month preparation plan*

## How to use this guide

1. Read one module's **core notes** and explain each term aloud without looking.
2. Memorise the comparison tables and algorithm steps.
3. Solve the numerical/practice questions on paper. Operating Systems exams commonly test both theory and calculations.
4. For a long-answer question, write: **definition → diagram/algorithm → example → advantages/limitations → conclusion**.

### Four-week priority plan

| Days | Work |
|---|---|
| 1–2 | Module 1; revise OS types, services and structures |
| 3–8 | Module 2; process states, threads, scheduling numericals |
| 9–14 | Module 3; semaphores and classical synchronization problems |
| 15–17 | Module 4; deadlock conditions and Banker’s algorithm |
| 18–22 | Module 5; paging, TLB, page replacement numericals |
| 23–26 | Module 6; I/O, disk scheduling and file systems |
| 27 | Solve one complete previous/model paper |
| 28 | Revise weak topics and formula/algorithm sheets |
| 29 | Solve a second paper under exam conditions |
| 30 | Light revision: definitions, diagrams, comparisons; sleep properly |

**High-scoring topics:** CPU scheduling, semaphores/classical problems, Banker’s algorithm, paging/page replacement, disk scheduling, file allocation, and comparisons between similar algorithms.

---

# Module 1 — Introduction to Operating Systems

## 1. What is an Operating System?

An **Operating System (OS)** is system software that acts as an intermediary between users/applications and computer hardware.

```
Users / applications
        ↓
Operating system (kernel + system programs)
        ↓
CPU, memory, disks, keyboard, network, etc.
```

The OS has two broad roles:

- **Resource manager:** allocates CPU time, memory, files and I/O devices among programs.
- **Control program:** controls execution, prevents errors and protects one program from another.

### Main goals

- Make the computer **easy to use**.
- Use hardware **efficiently**.
- Allow several programs/users to work safely at the same time.
- Provide **protection, security and reliability**.

The **kernel** is the privileged core of the OS. It remains available while the computer is running and performs tasks such as scheduling, memory management, device control and system-call handling.

## 2. Evolution of operating systems

1. **Serial processing:** one job was manually loaded and run at a time; very poor CPU utilisation.
2. **Simple batch systems:** jobs were collected into batches and executed automatically. There was no interaction while a job ran.
3. **Multiprogramming:** several jobs were kept in memory. When one waited for I/O, the CPU ran another job.
4. **Time-sharing/multitasking:** the CPU rapidly switches between interactive users/programs using time slices, giving the impression of simultaneous execution.
5. **Personal-computer OS:** designed for usability, graphics, devices and individual users.
6. **Network/distributed systems:** computers communicate and share resources over a network.
7. **Modern systems:** mobile, cloud, multicore, virtualised and embedded/real-time systems.

## 3. Types of operating systems

| Type | Meaning / use |
|---|---|
| Batch OS | Executes grouped jobs with little/no user interaction. |
| Multiprogramming OS | Keeps multiple jobs in memory to keep the CPU busy. |
| Time-sharing OS | Gives interactive users/programs small CPU time slices. |
| Multiprocessing OS | Uses two or more CPUs/cores; improves throughput and reliability. |
| Distributed OS | Coordinates several networked computers and may present them as one system. |
| Network OS | Provides network services such as file/printer sharing; machines remain visible separately. |
| Real-time OS (RTOS) | Must respond within timing constraints. Hard RTOS cannot miss deadlines; soft RTOS tries to meet them. |
| Embedded OS | Small, specialised OS inside appliances, vehicles and controllers. |
| Mobile OS | Supports touch interfaces, battery management, sensors and wireless communication. |

## 4. Structural overview

### User mode and kernel mode

Modern CPUs provide at least two execution modes:

- **User mode:** application code runs with restricted privileges.
- **Kernel mode:** OS code can execute privileged instructions and access hardware.

An application requests an OS service through a **system call**. The system call changes execution to kernel mode; after the OS completes the request, control returns to user mode. This protects hardware and other processes.

### Common OS structures

- **Monolithic kernel:** most OS services run in one large kernel. Fast communication, but a bug can affect the whole system.
- **Layered system:** OS is divided into layers; each layer uses lower layers. Easier to design/debug, but strict layers can add overhead.
- **Microkernel:** only essential functions (for example, scheduling, basic memory and IPC) remain in the kernel; other services run as user processes. Better isolation, but communication may add overhead.
- **Modular/hybrid kernel:** a small core with loadable modules or selected services. Offers a practical balance of speed and flexibility.
- **Virtual-machine structure:** a virtualisation layer provides several virtual computers over one physical machine.

## 5. Operating system services

1. **User interface:** command-line, graphical or touch interface.
2. **Program execution:** load a program, run it and terminate it.
3. **I/O operations:** provide a safe standard way to use devices.
4. **File-system manipulation:** create, delete, open, close, read, write and organise files/directories.
5. **Communication:** exchange data between processes on one computer or across a network.
6. **Error detection:** detect CPU, memory, I/O and program errors.
7. **Resource allocation:** assign CPU, memory, files and devices.
8. **Accounting:** record resource usage for monitoring or billing.
9. **Protection and security:** control who may access which resource; authentication verifies identity.

### Important distinction

- **System call:** the programming interface used by a program to request a kernel service (`open`, `read`, `fork`, `exec`, `wait`, etc.).
- **Interrupt:** a signal, usually from hardware, that asks the CPU to handle an event.
- **Exception/trap:** a CPU-generated event caused by an instruction, such as divide-by-zero or a system-call request.

### Module 1 quick questions

- Define an operating system. Explain its two major roles.
- Explain the evolution from batch processing to time-sharing.
- Compare batch, multiprogramming, time-sharing and real-time operating systems.
- Explain user mode, kernel mode and system calls with a diagram.
- Compare monolithic, layered and microkernel structures.
- List and explain operating system services.
- Differentiate multiprogramming, multitasking, multiprocessing and multithreading.

---

# Module 2 — Processes, Threads and CPU Scheduling

## 1. Process concept

A **program** is a passive file containing instructions. A **process** is a program currently being executed, together with its current state, memory and allocated resources.

A process may contain:

- code/text section,
- data and heap,
- stack (function calls/local variables),
- program counter (next instruction),
- CPU registers,
- open files and allocated devices.

### Process states

```
             admitted
   New --------------------> Ready
                              |
                         dispatch
                              v
                         Running --------> Terminated
                         /   |  \
             I/O/event /    |   \ preemption
                       v     |    \
                   Waiting --      -> Ready
                    (blocked)
                       |
                 event completed
                       └------------> Ready
```

- **New:** being created.
- **Ready:** able to run, waiting for the CPU.
- **Running:** currently executing on a CPU.
- **Waiting/Blocked:** waiting for I/O or an event.
- **Terminated:** finished or killed.
- **Suspended-ready:** not currently in main memory but ready to run after being brought back.
- **Suspended-blocked:** swapped out and also waiting for an event.

A process changes state because of events such as dispatch, I/O request, I/O completion, timer expiry, creation or termination.

## 2. Process Control Block (PCB)

A **PCB** is the OS record for one process. It commonly contains:

- process identifier (PID),
- process state,
- program counter,
- CPU registers and stack pointer,
- CPU-scheduling information (priority, queue pointers),
- memory-management information (page tables, limits),
- accounting information (CPU time, owner),
- I/O status and list of open files.

During a **context switch**, the OS saves the running process's CPU context in its PCB and loads another process's saved context. Context-switch time is overhead: no user program work is completed during it.

### Schedulers and dispatcher

- **Long-term/job scheduler:** decides which jobs enter the system; controls degree of multiprogramming.
- **Short-term/CPU scheduler:** chooses the next ready process frequently.
- **Medium-term scheduler:** suspends/swaps processes out and later brings them back.
- **Dispatcher:** gives the selected process the CPU, changes to user mode and jumps to its next instruction.

## 3. Independent and cooperating processes

- **Independent process:** cannot affect or be affected by another process's execution/data.
- **Cooperating process:** can share data or communicate with other processes. Cooperation improves information sharing, speed, modularity and convenience, but creates synchronisation problems.

### Process interaction with the OS

A process uses system calls to be created, scheduled, paused, resumed, synchronised, given resources and terminated. A typical UNIX-like sequence is:

- `fork()` creates a new process,
- `exec()` replaces a process's memory with a new program,
- `wait()` lets a parent wait for a child,
- `exit()` terminates a process.

## 4. Threads

A **thread** is the smallest unit of CPU execution within a process. Threads of one process share code, data, heap and open files, but each thread has its own program counter, registers and stack.

### Benefits

- **Responsiveness:** one thread can continue while another waits for I/O.
- **Resource sharing:** threads naturally share a process's memory.
- **Economy:** creating/switching threads is usually cheaper than processes.
- **Scalability:** threads can run in parallel on multicore CPUs.

Because shared memory is easy to access, threads must use synchronisation to avoid race conditions.

### User-level and kernel-level threads

| Feature | User-level threads | Kernel-level threads |
|---|---|---|
| Managed by | User library | OS kernel |
| Creation/switch | Fast; no kernel mode required | More overhead |
| Blocking system call | In many-to-one model, can block the whole process | Kernel can schedule another thread |
| Multicore parallelism | Not possible in many-to-one model | Supported |
| OS awareness | Kernel may see only one process | Kernel schedules individual threads |

Thread mapping models:

- **Many-to-one:** many user threads map to one kernel thread; fast but no true parallelism and one blocking call may block all.
- **One-to-one:** each user thread maps to a kernel thread; parallelism and blocking support, but creation limits/overhead.
- **Many-to-many:** many user threads are multiplexed over several kernel threads; flexible but complex.

## 5. CPU scheduling

The scheduler selects a ready process for the CPU.

### Scheduling criteria

- **CPU utilisation:** percentage of time the CPU is busy; maximise it.
- **Throughput:** completed processes per unit time; maximise it.
- **Turnaround time:** `Completion time − Arrival time`; minimise it.
- **Waiting time:** total time in the ready queue; minimise it.
- **Response time:** `First CPU start time − Arrival time`; minimise it for interactive systems.
- **Fairness/predictability:** avoid starvation and give reasonable service.

If all processes are given the same priority, use a clear tie-breaking rule such as arrival order or PID.

### Preemptive vs non-preemptive scheduling

- **Non-preemptive:** once a process gets the CPU, it keeps it until it terminates or blocks. Simple and low overhead, but a long job can delay all others.
- **Preemptive:** OS can interrupt a running process, often using a timer, and give the CPU to another. Better response, but has context-switch overhead and needs careful synchronisation.

### Algorithms

#### 1. First-Come, First-Served (FCFS)

Runs processes in arrival order. It is non-preemptive and simple.

- Good: simple, no starvation.
- Bad: **convoy effect**—a long CPU-bound process makes many short processes wait.

#### 2. Shortest Job First (SJF)

Chooses the ready process with the smallest next CPU burst. It is normally non-preemptive.

- Gives the minimum average waiting time when burst lengths are known/estimated.
- Long jobs may starve.

#### 3. Shortest Remaining Time First (SRTF)

Preemptive SJF. A newly arriving process preempts the current one if its remaining time is shorter.

- Often better response/average waiting than SJF.
- More context switches; starvation is possible.

#### 4. Round Robin (RR)

Each ready process receives a time quantum `q` in circular order.

- Small `q`: responsive but many context switches.
- Large `q`: behaves like FCFS.
- A good time quantum should be much larger than the context-switch time, while keeping response acceptable.

#### 5. Priority scheduling

The highest-priority ready process runs. It may be preemptive or non-preemptive. Decide whether a smaller number or larger number means higher priority.

- Problem: **starvation** of low-priority processes.
- **Aging:** gradually raises the priority of a waiting process to prevent starvation.

#### 6. Multilevel queue (MLQ)

The ready queue is permanently divided, for example foreground/interactive and background/batch. Each queue may use a different algorithm, and queues may have fixed priority or a CPU share.

- Fast and suitable for different classes.
- A process generally cannot move to another queue; lower queues may starve.

#### 7. Multilevel feedback queue (MLFQ)

Like MLQ, but processes can move between queues based on behaviour. CPU-bound jobs move down; interactive/I/O-bound jobs often remain high.

Typical rules: new jobs enter the top queue, a job using its full quantum moves down, and aging moves long-waiting jobs up.

- Flexible and approximates SJF without knowing burst times.
- More complicated to configure and analyse.

### Real-time scheduling

A real-time task has a computation time, period/deadline and timing requirement.

- **Rate Monotonic (RM):** fixed priority; shorter period means higher priority. It is suitable for periodic tasks. A task with period `T1 < T2` gets higher priority than the second.
- **Earliest Deadline First (EDF):** dynamic priority; the task with the nearest absolute deadline runs first. On an ideal single CPU, EDF can schedule more feasible periodic work than RM, but priorities change at runtime.

### CPU scheduling numerical method

For each process, draw a **Gantt chart**. Then calculate:

```
Turnaround time = Completion time − Arrival time
Waiting time    = Turnaround time − CPU burst time
Response time   = First start time − Arrival time
Average         = Sum of values / Number of processes
```

Example: `P1(AT=0,BT=5), P2(AT=1,BT=3), P3(AT=2,BT=1)`.

- FCFS order: `P1 | P2 | P3` (0–5, 5–8, 8–9). Waiting times: `0, 4, 6`; average = `3.33`.
- Non-preemptive SJF order: `P1 | P3 | P2`. Waiting times: `0, 3, 5`; average = `2.67`.
- SRTF order: `P1(0–1) | P2(1–2) | P3(2–3) | P2(3–6) | P1(6–10)`. Waiting times: `5, 2, 0`; average = `2.33`.

Always consider arrival times; never run a process before it arrives.

## 6. Message-passing IPC

In **message passing**, processes communicate using `send(message)` and `receive(message)` rather than directly sharing memory.

- **Direct communication:** sender names the receiver, e.g. `send(P, m)`.
- **Indirect communication:** messages go through a mailbox/port.
- **Synchronous/blocking:** sender or receiver waits until the operation completes.
- **Asynchronous/non-blocking:** operation returns immediately; a queue stores messages.
- **Zero-capacity buffer:** sender waits for a receiver (rendezvous).
- **Bounded buffer:** queue has finite capacity; sender may wait when full.
- **Unbounded buffer:** conceptually no sender waiting for space.

Message passing is useful across machines and gives isolation, but it can be slower than shared memory because of copying and system calls.

### Module 2 important questions

**Long answers:**

- Draw and explain process states and transitions.
- Explain PCB and context switching.
- Explain long-, medium- and short-term schedulers and the dispatcher.
- Compare process and thread; explain benefits and thread mapping models.
- Solve a CPU scheduling problem using FCFS, SJF, SRTF, RR and priority, and calculate averages.
- Compare MLQ and MLFQ.
- Explain RM and EDF real-time scheduling.
- Explain message passing, its addressing and synchronisation choices.

**Short answers:** program vs process, ready vs waiting, suspended process, response vs turnaround time, aging, convoy effect, starvation, user vs kernel threads.

---

# Module 3 — Inter-Process Communication and Synchronisation

## 1. Why synchronisation is needed

Processes/threads may share data. A **race condition** occurs when the result depends on the order in which concurrent operations execute.

Example: if two threads both execute `counter = counter + 1`, the read-modify-write sequence can lose one update.

A **critical section** is the part of a process that accesses shared data/resources.

A correct solution must satisfy:

1. **Mutual exclusion:** at most one process is in the critical section.
2. **Progress:** if no one is in the critical section, selection of the next entrant cannot be postponed forever.
3. **Bounded waiting:** a process requesting entry will not wait forever; this prevents starvation.

General structure:

```text
entry section
critical section
exit section
remainder section
```

## 2. Synchronisation hardware

Hardware provides atomic (indivisible) instructions such as:

- **Test-and-set:** atomically reads a lock and sets it.
- **Compare-and-swap (CAS):** changes a value only if it still equals an expected old value.
- **Exchange:** atomically swaps register and memory values.
- **Atomic fetch-and-add:** useful for counters/ticket locks.

A **spinlock** repeatedly checks a lock. It is useful for very short waits on multiprocessors, but wastes CPU time for long waits. Disabling interrupts can protect a kernel critical section on a single CPU, but is unsafe for user programs and does not scale well to multiprocessors.

## 3. Peterson’s solution

Peterson’s algorithm solves the two-process critical-section problem using shared variables:

```text
boolean flag[2] = {false, false};
int turn;

Process i (j is the other process):
    flag[i] = true;
    turn = j;
    while (flag[j] && turn == j) ;   // wait

    // critical section

    flag[i] = false;
    // remainder section
```

`flag[i]` says process `i` wants to enter; `turn` gives the other process preference when both want to enter. It provides mutual exclusion, progress and bounded waiting under suitable memory-order assumptions. It is mainly a teaching algorithm; real systems use atomic hardware primitives.

## 4. Semaphores

A **semaphore** is an integer synchronisation variable accessed only through atomic operations:

- `wait(S)` / `P(S)`: decrement; if no resource is available, block.
- `signal(S)` / `V(S)`: increment; wake a waiting process if appropriate.

Types:

- **Binary semaphore:** values 0/1; can protect a critical section.
- **Counting semaphore:** represents several identical resources.

Do not confuse a semaphore with a normal integer: its operations must be atomic.

### Producer–consumer (bounded buffer)

For a buffer of size `N`:

```text
semaphore mutex = 1;   // protects buffer
semaphore empty = N;   // empty slots
semaphore full = 0;    // filled slots
```

Producer:

```text
produce item
wait(empty)
wait(mutex)
    insert item
signal(mutex)
signal(full)
```

Consumer:

```text
wait(full)
wait(mutex)
    remove item
signal(mutex)
signal(empty)
consume item
```

The order matters. For example, taking `mutex` before waiting for an empty slot can deadlock if the buffer is full because the consumer cannot obtain the mutex to create an empty slot.

## 5. Classical synchronisation problems

### Readers–writers problem

Many readers may read shared data simultaneously, but a writer needs exclusive access.

- **Reader-preference solution:** readers can enter while other readers are present. It can starve writers.
- **Writer-preference solution:** once a writer is waiting, new readers wait. It prevents writer starvation but may delay readers.
- A fair solution uses a queue/order mechanism to prevent starvation of either side.

A typical semaphore design uses `rw_mutex` for the shared data and `mutex` to protect `read_count`; the first reader locks `rw_mutex`, and the last reader releases it.

### Dining philosophers problem

Five philosophers alternate between thinking and eating. Each needs two forks; a naive solution where everyone picks up the left fork first can cause circular wait/deadlock.

Solutions:

- allow at most four philosophers to try to eat simultaneously,
- use a waiter/monitor that grants both forks together,
- impose an ordering on forks and always acquire the lower-numbered fork first,
- make one philosopher pick up forks in the opposite order.

A good solution should avoid both deadlock and starvation.

### Monitors

A **monitor** is a high-level synchronisation construct containing shared data, procedures and an implicit lock. Only one process/thread executes inside a monitor at a time.

**Condition variables** provide `wait()` and `signal()`:

- `wait(condition)`: releases the monitor lock and sleeps.
- `signal(condition)`: wakes one waiting thread, if any.

Monitors are safer and easier to use than manually placing semaphore operations, but the language/runtime must support them.

### Module 3 important questions

- Define race condition and critical section. State the three requirements for a correct solution.
- Explain test-and-set, CAS and spinlocks.
- Write and explain Peterson’s solution.
- Define semaphore and distinguish binary and counting semaphores.
- Solve the bounded-buffer problem using semaphores; explain why operation order matters.
- Explain readers–writers and discuss reader/writer starvation.
- Explain the dining philosophers problem and at least three solutions.
- Explain monitors and condition variables.
- Compare shared-memory IPC and message-passing IPC.

---

# Module 4 — Deadlocks

## 1. Definition and example

A **deadlock** is a state in which a set of processes is permanently blocked because each process is waiting for a resource/event that another process in the set must provide.

Example: `P1` holds printer A and waits for printer B; `P2` holds printer B and waits for printer A.

A resource-allocation graph has process nodes, resource nodes, request edges (`P → R`) and assignment edges (`R → P`). For single-instance resources, a cycle indicates deadlock.

## 2. Four necessary conditions

All four must hold for a traditional resource deadlock:

1. **Mutual exclusion:** a resource is non-shareable; only one process uses it at a time.
2. **Hold and wait:** a process holds at least one resource while waiting for another.
3. **No preemption:** resources cannot be forcibly taken; they must be released voluntarily.
4. **Circular wait:** a circular chain exists where each process waits for a resource held by the next.

Removing any one condition prevents deadlock. Under the usual assumptions, the four conditions together are necessary and sufficient for deadlock.

## 3. Methods for handling deadlocks

### A. Prevention

Design the system so at least one necessary condition never holds:

- make resources sharable where possible (not possible for every resource),
- require a process to request all resources at once (breaks hold-and-wait but lowers utilisation),
- allow preemption where possible,
- impose a total ordering on resource types and require requests in increasing order (breaks circular wait).

### B. Avoidance

The OS needs advance information about each process's maximum resource demand. Before granting a request, it checks whether the new state is **safe**.

- **Safe state:** there exists a sequence in which every process can obtain its remaining resources, finish and release resources.
- **Unsafe state:** no such guarantee; it is not necessarily already deadlocked, but granting more may lead to deadlock.

### Banker’s algorithm

Data structures:

- `Available`: currently free units of each resource.
- `Max[i]`: maximum demand of process `i`.
- `Allocation[i]`: resources currently held by `i`.
- `Need[i] = Max[i] − Allocation[i]`.

**Safety algorithm:**

1. Set `Work = Available`; set all `Finish[i] = false`.
2. Find an unfinished process `i` with `Need[i] ≤ Work`.
3. Pretend it finishes: `Work = Work + Allocation[i]`; set `Finish[i] = true`.
4. Repeat. If every process finishes, the state is safe; otherwise it is unsafe.

**Resource-request algorithm:** for a request by `Pi`, first check `Request ≤ Need[i]` and `Request ≤ Available`. Temporarily allocate it, run the safety test, and grant only if the result is safe.

Classic safe-state example:

```text
Available = (3, 3, 2)

             Allocation   Max          Need
P0           (0,1,0)      (7,5,3)      (7,4,3)
P1           (2,0,0)      (3,2,2)      (1,2,2)
P2           (3,0,2)      (9,0,2)      (6,0,0)
P3           (2,1,1)      (2,2,2)      (0,1,1)
P4           (0,0,2)      (4,3,3)      (4,3,1)
```

A safe sequence is `P1 → P3 → P4 → P0 → P2`. After each process finishes, add its Allocation back to Work.

### C. Detection

Allow deadlocks, then periodically detect them:

- For single-instance resources, cycle detection in a wait-for graph is useful.
- For multiple instances, use an algorithm similar to the Banker's safety test, treating current requests and available resources as the detection input.

Detection frequency is a trade-off: frequent checks detect sooner but cost more.

### D. Recovery

After detecting deadlock:

- **Terminate processes:** abort all deadlocked processes, or abort one at a time until the cycle disappears.
- **Resource preemption:** take a resource from a process, roll it back and give it to another. Need victim selection and starvation prevention.
- **Rollback/restart:** restore a process to a safe checkpoint.

### Deadlock vs starvation vs livelock

- **Deadlock:** processes wait forever for one another.
- **Starvation:** one process waits indefinitely because others keep being selected first.
- **Livelock:** processes keep changing state/responding but make no useful progress.

### Module 4 important questions

- Define deadlock and explain the four necessary conditions with an example.
- Explain resource-allocation and wait-for graphs.
- Compare prevention, avoidance, detection and recovery.
- Solve a Banker’s algorithm safety problem and find a safe sequence.
- Test whether a resource request can be granted safely.
- Explain deadlock recovery techniques.
- Differentiate deadlock, starvation and livelock.

---

# Module 5 — Memory Management and Virtual Memory

## 1. Logical and physical address spaces

- **Logical (virtual) address:** generated by the CPU for a process.
- **Physical address:** actual location in main memory (RAM).
- **MMU (Memory Management Unit):** hardware that translates logical addresses into physical addresses.

Each process can have its own protected logical address space. The OS and MMU prevent an ordinary process from accessing another process's memory.

## 2. Swapping

**Swapping** moves a process between main memory and backing store (disk) to free RAM. It supports more processes than can fit in memory, but disk is slow, so excessive swapping severely reduces performance.

## 3. Contiguous memory allocation

Each process occupies one continuous range of physical memory.

- **Fixed partitions:** memory divided beforehand; easy but causes internal fragmentation.
- **Variable partitions:** allocate a suitably sized region for each process; can cause external fragmentation.

Placement strategies:

- **First fit:** first hole large enough; fast.
- **Best fit:** smallest suitable hole; may create many tiny holes.
- **Worst fit:** largest hole; leaves a large remainder but often wastes space.

**Internal fragmentation:** allocated block contains unused space inside it.

**External fragmentation:** enough total free memory exists, but it is split into non-contiguous holes.

**Compaction** moves processes to combine holes, but costs time and requires relocation support.

## 4. Paging

Paging divides:

- logical memory into fixed-size **pages**,
- physical memory into equal-size **frames**.

A logical address is divided into:

```text
page number p | offset d
```

The page table maps page `p` to frame `f`. The physical address is:

```text
frame number f | same offset d
```

If page size is `2^d` bytes, the offset uses `d` bits. If logical address size is `m` bits, page number bits are `m − d`.

Advantages: eliminates external fragmentation and supports non-contiguous allocation. Costs: page-table memory, address-translation time and possible internal fragmentation in the last page.

Page-table improvements include multilevel page tables, hashed/inverted tables and a TLB.

## 5. Segmentation

Segmentation divides a program into logical variable-size units such as code, data, stack and procedures. A logical address is `(segment number, offset)`.

A segment table stores a **base** and **limit**. The OS checks `offset < limit`; physical address = `base + offset`.

- Good for protection and logical sharing.
- Can suffer from external fragmentation.

### Paging vs segmentation

| Paging | Segmentation |
|---|---|
| Fixed-size physical units | Variable-size logical units |
| Mostly invisible to programmer | Matches program modules |
| Avoids external fragmentation | Can have external fragmentation |
| Address: page + offset | Address: segment + offset |

## 6. Translation Lookaside Buffer (TLB)

A **TLB** is a fast associative cache of recent page-table entries.

- TLB hit: page-to-frame mapping is found quickly.
- TLB miss: page table in memory must be consulted; then the entry may be placed in TLB.

With TLB lookup time `t`, memory access time `m`, and hit ratio `h`, a common one-level formula is:

```text
Effective access time = h(t + m) + (1 − h)(t + 2m)
```

This assumes a page-table memory access is needed on a miss and ignores page faults.

## 7. Virtual memory and demand paging

**Virtual memory** allows a process to run even when its entire address space is not in RAM. Only needed pages are loaded.

In **demand paging**, a page is brought into memory only when referenced.

### Page fault steps

1. CPU references a page whose valid bit is 0.
2. Hardware traps to the OS.
3. OS checks whether the reference is legal.
4. OS finds a free frame, or selects a victim page.
5. If the victim is dirty, write it to disk.
6. Read the required page from backing store.
7. Update page table/TLB and valid bit.
8. Restart the interrupted instruction.

A page fault is extremely slower than a normal memory access, so the page-fault rate must be kept low.

## 8. Page-replacement algorithms

When all frames are full, a page must be replaced. Count a **page fault** whenever the referenced page is not already in a frame.

### FIFO / FCFS

Replace the page that entered memory first. It is simple, but may show **Belady’s anomaly**: increasing the number of frames can increase faults.

### LRU (Least Recently Used)

Replace the page that has not been used for the longest time. It uses recent history and generally performs well, but exact implementation needs hardware support/counters/stack approximations.

### Optimal

Replace the page whose next use is farthest in the future. It gives the minimum possible faults for a known reference string, so it is mainly a benchmark; the future is not known in real systems.

**Exam method:** draw columns for each frame, process the reference string left to right, mark hit/fault, and apply only the chosen replacement rule.

## 9. Thrashing and working set

**Thrashing** occurs when the system spends most of its time handling page faults and swapping rather than executing useful instructions. It can happen when processes have too few frames or the degree of multiprogramming is too high.

The **working set** of a process is the set of pages referenced during a recent window of `Δ` memory references/time. If the total working-set demand of all processes exceeds available frames, reduce multiprogramming or allocate more frames. This is the working-set model's way of controlling thrashing.

### Module 5 important questions

- Differentiate logical and physical addresses; explain the MMU.
- Explain swapping and its performance cost.
- Solve a first-fit/best-fit/worst-fit allocation problem and identify fragmentation.
- Explain paging with address translation and a page-table diagram.
- Compare paging and segmentation.
- Explain TLB and calculate effective access time.
- Explain demand paging and the page-fault handling sequence.
- Solve page-replacement problems using FIFO/FCFS, LRU and Optimal.
- Compare FIFO, LRU and Optimal; explain Belady’s anomaly.
- Define thrashing and explain the working-set model.

---

# Module 6 — I/O, Disk Management and File Systems

## 1. I/O hardware

An I/O device communicates through a **device controller**, which contains control/status registers, data registers and device-specific logic. The OS usually uses a device driver to give a uniform interface to different hardware.

### I/O techniques

#### Polling (programmed I/O)

The CPU repeatedly checks a device status register until the device is ready.

- Simple, predictable.
- Wastes CPU time while waiting.

#### Interrupt-driven I/O

The CPU starts the operation and does other work. The device raises an interrupt when it needs service or finishes.

- Better CPU utilisation.
- Interrupt handling has overhead; too many interrupts can overload the CPU.

#### Direct Memory Access (DMA)

For a large transfer, the CPU gives the DMA controller the source/destination, count and direction. DMA moves a block directly between device and memory; the CPU is usually interrupted once per block.

- Excellent for high-speed/block transfers.
- Requires setup and careful cache coherence/protection.

### Caching, buffering and spooling

- **Caching:** keep copies of frequently used data in faster storage; improves speed but may create consistency issues.
- **Buffering:** use temporary memory while producer and consumer operate at different speeds; handles bursts and absorbs timing differences.
- **Spooling:** queue data for a device that serves one request at a time, such as a printer; disk acts as a large buffer.

### Blocking and non-blocking I/O

- **Blocking I/O:** the calling process sleeps until the operation completes.
- **Non-blocking I/O:** the call returns immediately; it may return available data, a status or an indication that the operation is in progress.

Non-blocking I/O can improve responsiveness but makes program design more complex.

## 2. Disk structure and access time

A magnetic disk has platters, surfaces, tracks, sectors and cylinders. A disk request may cost:

```text
Disk access time = seek time + rotational latency + transfer time + controller overhead
```

- **Seek time:** move the disk arm to the required track.
- **Rotational latency:** wait for the sector to rotate under the head.
- **Transfer time:** move the data.

Disk scheduling mainly tries to reduce head movement/seek time while preserving fairness.

## 3. Disk-scheduling algorithms

Given a current head position, requests and disk limits, draw the head movement in order and add absolute differences between consecutive positions.

### FCFS

Serve requests in arrival order.

- Fair and simple.
- May cause large, inefficient head movement.

### SSTF (Shortest Seek Time First)

Serve the request nearest to the current head position.

- Usually less movement than FCFS.
- Far-away requests may starve.

### SCAN (elevator)

Move in one direction, serving requests along the path; on reaching the end, reverse direction.

- More predictable than SSTF.
- Whether the physical end is counted depends on the exact convention in the question; state your assumption.

### C-SCAN

Serve requests in one direction only. At the end, jump to the other end without servicing on the return, then continue in the same direction.

- More uniform waiting time than SCAN.
- Extra movement for the jump.

### LOOK

Like SCAN, but reverse at the last pending request in that direction rather than travelling to the physical end.

### C-LOOK

Like C-SCAN, but jump from the last request in one direction to the first request at the other side; it does not travel to the physical ends unnecessarily.

**Exam warning:** for SCAN/C-SCAN, write the initial direction and disk range. Different assumptions change the total movement.

## 4. Disk reliability and formatting

Reliability measures include error-detecting/correcting codes, checksums, redundant copies/RAID, regular backups, bad-sector remapping and monitoring.

- **Low-level/physical formatting:** divides a disk surface into sectors and records control information; usually done by the manufacturer.
- **Partitioning:** divides a disk into logical regions.
- **Logical formatting:** creates a file-system structure such as metadata, directories and free-space information.

### Boot block

The **boot block** (or boot sector) contains code/information needed to start loading the OS. Firmware finds it during boot; a boot loader then loads the kernel. Many systems keep boot code in a special location even if the OS file system is elsewhere.

### Bad blocks

Bad blocks are damaged/unreliable storage sectors. The system can:

- maintain a bad-block list,
- remap a bad sector to a spare sector,
- use sector error correction,
- avoid allocating known bad blocks.

## 5. File concept and access methods

A **file** is a named collection of related data stored on secondary storage. File attributes may include name, identifier, type, location, size, protection, owner and timestamps.

Common operations: create, open, read, write, seek/reposition, close, delete, truncate and rename.

Access methods:

- **Sequential access:** read/write records in order; good for tapes and logs.
- **Direct/random access:** jump to a block/offset; useful for disks and databases.
- **Indexed access:** an index maps keys/record numbers to data locations.

## 6. Directory structures

- **Single-level:** one directory for all files; simple but name conflicts occur.
- **Two-level:** separate directory per user; reduces conflicts.
- **Tree-structured:** directories can contain subdirectories; common and scalable.
- **Acyclic graph:** permits shared files/directories through links but disallows cycles.
- **General graph:** cycles are possible; requires cycle detection or garbage collection.

## 7. File-system structure

A typical file system has layers:

```text
Applications
  ↓
Logical file system (names, directories, protection)
  ↓
File-organization module (allocation, free space)
  ↓
Basic file system (blocks and buffers)
  ↓
I/O control + device drivers
  ↓
Storage hardware
```

Important on-disk structures include:

- boot control block,
- volume/superblock containing file-system parameters,
- directory structures,
- file control blocks/inodes,
- data blocks,
- free-space map/bit vector.

## 8. UNIX file structure

UNIX-like systems use a hierarchical tree and a **file descriptor** for an open file.

- A directory maps a filename to an **inode number**.
- The inode stores metadata (owner, permissions, size, timestamps, link count) and pointers to data blocks; the filename is normally stored in the directory entry.
- `open()` returns a file descriptor; `read`, `write`, `lseek` and `close` operate through it.
- Directories are treated as files containing name-to-inode mappings.
- Hard links point to the same inode; symbolic links store a path to another file.
- Permissions are commonly represented for owner, group and others, with read/write/execute bits.

## 9. File-allocation methods

### Contiguous allocation

A file occupies consecutive disk blocks; metadata stores starting block and length.

- Very fast sequential and direct access.
- External fragmentation and difficult file growth.

### Linked allocation

Each file block contains a pointer to the next block; blocks can be anywhere.

- No external fragmentation; easy growth.
- Poor direct access and pointer overhead; a damaged pointer can lose the rest of a file.

### Indexed allocation

An index block contains pointers to all data blocks (or uses multilevel indexes).

- Supports direct access and no external fragmentation.
- Index-block space overhead; small files may waste index space.

| Method | Main advantage | Main disadvantage |
|---|---|---|
| Contiguous | Fastest/simple access | Fragmentation and growth problem |
| Linked | Easy growth, no external fragmentation | Slow random access, pointer overhead |
| Indexed | Direct access and flexible placement | Index overhead |

## 10. Free-space management — bit vector

A **bit vector/bitmap** has one bit per disk block:

- `0` may mean free and `1` allocated (or the reverse; state the convention).
- To allocate `n` contiguous blocks, search for `n` consecutive free bits.
- To release blocks, clear their bits.

Advantages: compact and easy to find runs of free blocks using bit operations. Disadvantage: a very large disk requires a large bitmap, though the bitmap is much smaller than a full block table.

### Module 6 important questions

- Explain polling, interrupt-driven I/O and DMA with comparison.
- Differentiate caching, buffering and spooling.
- Compare blocking and non-blocking I/O.
- Explain disk structure and disk access time.
- Solve disk-scheduling problems using FCFS, SSTF, SCAN, C-SCAN, LOOK and C-LOOK.
- Explain disk reliability, formatting, boot blocks and bad-block handling.
- Define a file and explain file attributes/operations/access methods.
- Explain directory structures with diagrams.
- Explain layered file-system structure and UNIX inodes/file descriptors.
- Compare contiguous, linked and indexed allocation.
- Explain free-space management using a bit vector.

---

# Fast comparison sheet

## Scheduling

| Algorithm | Preemptive? | Strength | Main problem |
|---|---:|---|---|
| FCFS | No | Simple/fair arrival order | Convoy effect |
| SJF | No | Minimum average waiting if burst known | Starvation; burst prediction |
| SRTF | Yes | Good average waiting/response | More overhead/starvation |
| RR | Yes | Fair and responsive | Quantum choice/overhead |
| Priority | Either | Important tasks first | Starvation; use aging |
| MLQ | Usually mixed | Separate job classes | Fixed queues/starvation |
| MLFQ | Yes | Adapts to behaviour | Complex tuning |
| RM | Fixed priority | Predictable periodic tasks | Lower utilisation than EDF in some cases |
| EDF | Dynamic priority | Earliest deadlines; flexible | Runtime priority management |

## Deadlock handling

| Method | Basic idea | Extra information needed? |
|---|---|---:|
| Prevention | Break a necessary condition | No maximum demand required |
| Avoidance | Grant only if state stays safe | Yes, maximum demands |
| Detection | Allow, then find deadlock | Current allocations/requests |
| Recovery | Remove detected deadlock | Victim/restart policy |

## Page replacement

| Algorithm | Rule | Reality |
|---|---|---|
| FIFO/FCFS | Oldest loaded page out | Simple; Belady anomaly possible |
| LRU | Least recently used out | Good; implementation cost |
| Optimal | Farthest future use out | Best benchmark; future unknown |

## IPC/synchronisation

| Technique | Core idea | Typical use |
|---|---|---|
| Shared memory | Processes access a shared region | Fast local data exchange; needs locks |
| Message passing | Send/receive messages | Isolation, distributed systems |
| Mutex/binary semaphore | One owner/one permission | Mutual exclusion |
| Counting semaphore | Count available units | Bounded resources/buffer slots |
| Monitor | Lock + condition variables | High-level structured synchronisation |

---

# Practice questions for the final week

## Theory

1. Explain all process states and draw their transitions.
2. What information does a PCB contain? Why is context switching overhead?
3. Compare user-level and kernel-level threads and explain the three mapping models.
4. State the critical-section requirements and explain Peterson’s algorithm.
5. Solve the bounded-buffer problem using `empty`, `full` and `mutex` semaphores.
6. Explain the four deadlock conditions and one method for breaking each.
7. Given `Available`, `Max` and `Allocation`, calculate `Need` and find a safe sequence using Banker’s algorithm.
8. Explain paging, address translation and the role of the TLB.
9. Compare FIFO, LRU and Optimal page replacement; explain Belady’s anomaly.
10. Explain page-fault handling and thrashing.
11. Compare polling, interrupts and DMA.
12. Compare SCAN, C-SCAN, LOOK and C-LOOK.
13. Explain UNIX inode structure and the difference between a hard link and a symbolic link.
14. Compare contiguous, linked and indexed file allocation.

## Numerical practice

1. For `P1(AT=0,BT=8)`, `P2(AT=1,BT=4)`, `P3(AT=2,BT=2)`, calculate Gantt charts and average waiting/turnaround/response time using FCFS, SJF and SRTF.
2. Using the same processes, solve Round Robin for time quantum `q=2`.
3. For a given page reference string and three frames, calculate faults using FIFO, LRU and Optimal. Mark every hit and replacement.
4. For a memory-hole list and process sizes, apply first fit, best fit and worst fit; calculate remaining holes and identify fragmentation.
5. Given a TLB hit ratio, TLB lookup time and memory access time, use `EAT = h(t+m)+(1-h)(t+2m)`.
6. For a disk queue and initial head, calculate total head movement under FCFS, SSTF, SCAN, C-SCAN, LOOK and C-LOOK. State the disk range and direction.
7. For a Banker’s table, calculate `Need = Max − Allocation`, test safety, then test a new request.

## Very short revision answers

- **Turnaround:** total time from arrival to completion.
- **Waiting:** time spent in the ready queue.
- **Response:** time until the process first receives CPU service.
- **Race condition:** output depends on concurrent execution order.
- **Semaphore:** atomic counter used for synchronisation/resource control.
- **Safe state:** at least one order exists in which all processes can finish.
- **Page fault:** referenced page is not currently in physical memory.
- **Thrashing:** most time is spent paging instead of executing.
- **DMA:** controller transfers a block between device and memory with little CPU involvement.
- **Inode:** UNIX metadata structure containing pointers to a file's data blocks.

---

# Practical CLI Command Reference

A **CLI (Command-Line Interface)** lets you control the OS by typing commands. Linux/macOS shells such as Bash and Zsh are especially important for OS, backend, cloud and DevOps work. The examples below use Bash syntax; many also work in WSL or Git Bash on Windows.

## 1. Shell fundamentals

| Command | Use | Example |
|---|---|---|
| `pwd` | Show the current directory | `pwd` |
| `ls` | List files | `ls -lah` (including hidden files, human-readable sizes) |
| `cd` | Change directory | `cd ~/projects/app` |
| `mkdir` | Create a directory | `mkdir -p src/components` |
| `touch` | Create an empty file/update timestamp | `touch notes.txt` |
| `cp` | Copy files/directories | `cp file.txt backup.txt`; `cp -r src/ backup/` |
| `mv` | Move or rename | `mv old.txt new.txt` |
| `rm` | Delete | `rm file.txt`; `rm -r folder/` |
| `ln` | Create links | `ln -s /path/to/target shortcut` |
| `clear` | Clear the terminal | `clear` |
| `history` | Show previous commands | `history 20` |
| `man` | Read a command's manual | `man chmod` |
| `command -v` | Find the executable used | `command -v python` |
| `file` | Identify file type | `file image.bin` |
| `stat` | Show file metadata | `stat config.json` |

Useful path shortcuts:

- `.` = current directory; `..` = parent; `~` = home directory.
- `/` = filesystem root; an absolute path starts with `/`; a relative path starts from the current directory.
- `*` matches multiple characters; `?` matches one character. Quote paths containing spaces: `cd "My Project"`.

**Safer deletion habit:** first run `ls` or `find` to verify the target. Be extremely careful with `rm -rf`; it permanently deletes recursively and does not use a recycle bin.

## 2. Read, search and transform text

| Command | Use | Example |
|---|---|---|
| `cat` | Print a small file | `cat package.json` |
| `less` | Scroll through a large file | `less /var/log/syslog` (`q` quits) |
| `head` / `tail` | First/last lines | `tail -n 50 app.log` |
| `tail -f` | Follow a growing log | `tail -f app.log` |
| `wc` | Count lines/words/bytes | `wc -l source.py` |
| `grep` | Search text by pattern | `grep -RIn "TODO" src/` |
| `rg` | Fast modern recursive search | `rg "error|timeout" .` |
| `find` | Search files by conditions | `find . -type f -name "*.js"` |
| `xargs` | Turn input into command arguments | `find . -name "*.tmp" -print0 \| xargs -0 rm` |
| `sort` / `uniq` | Sort and remove adjacent duplicates | `sort names.txt \| uniq -c` |
| `cut` | Select columns/characters | `cut -d: -f1 /etc/passwd` |
| `awk` | Process column-oriented text | `awk '{print $1, $3}' access.log` |
| `sed` | Stream editing/substitution | `sed 's/dev/prod/g' config.txt` |
| `tee` | Show and save output | `make 2>&1 \| tee build.log` |
| `jq` | Query/format JSON | `curl -s https://api.example.com/items \| jq '.items[]'` |

`grep` is universally available; `rg` (ripgrep) is generally faster and respects `.gitignore`. `awk` and `sed` are powerful but test a command on a copy before editing real files.

### Pipes and redirection

```bash
command1 | command2                 # send output of command1 to command2
command > output.txt                # overwrite a file with standard output
command >> output.txt               # append standard output
command 2> errors.txt               # redirect standard error
command > all.log 2>&1              # save output and errors together
command &> all.log                  # Bash shorthand for both
command1 && command2                # run command2 only if command1 succeeds
command1 || command2                # run command2 only if command1 fails
```

Example:

```bash
ps aux | grep '[n]ginx'              # find nginx processes without matching grep itself
find . -type f -name '*.log' -print0 | xargs -0 grep -n 'ERROR'
```

The special variable `$?` contains the previous command's exit status; `0` normally means success.

## 3. Processes, jobs and OS diagnostics

These commands connect directly to your OS syllabus topics: processes, CPU, memory, I/O and scheduling.

| Command | Use | Example |
|---|---|---|
| `ps` | Snapshot of processes | `ps aux`; `ps -ef --forest` |
| `top` | Live CPU/memory/process view | `top` |
| `htop` | Friendlier interactive process viewer | `htop` |
| `pgrep` / `pkill` | Find/ signal by name | `pgrep -af python`; `pkill -TERM app` |
| `kill` | Send a signal to a PID | `kill -TERM 1234` |
| `jobs` / `bg` / `fg` | Manage shell jobs | `jobs`; `bg %1`; `fg %1` |
| `nohup` | Continue after logout | `nohup ./server >server.log 2>&1 &` |
| `nice` / `renice` | Start/change CPU priority | `nice -n 10 command`; `renice 10 -p 1234` |
| `free` | RAM and swap usage | `free -h` |
| `uptime` | Running time and load average | `uptime` |
| `vmstat` | CPU, memory, paging and process statistics | `vmstat 1` |
| `iostat` | CPU and disk I/O statistics | `iostat -xz 1` |
| `df` | Free space by filesystem | `df -h` |
| `du` | Space used by files/directories | `du -sh * \| sort -h` |
| `lsof` | Files/sockets opened by processes | `lsof -i :8080` |
| `dmesg` | Kernel/device messages | `dmesg -T \| tail -n 50` |
| `uname` | Kernel/system information | `uname -a` |
| `watch` | Repeat a command | `watch -n 2 'free -h'` |
| `time` | Measure command duration | `/usr/bin/time -v ./program` |

Signals commonly used with `kill`:

- `TERM` (15): polite request to terminate; the program can clean up.
- `INT` (2): interrupt, usually like pressing `Ctrl+C`.
- `KILL` (9): force termination; cannot be handled or cleaned up. Use only when `TERM` fails.
- `HUP` (1): traditionally hang-up; many daemons use it to reload configuration.

A PID is a process identifier. Avoid killing a process by name on production machines until you have verified the exact PID with `ps` or `pgrep`.

### Services and logs on Linux

```bash
systemctl status nginx                  # service state
sudo systemctl restart nginx             # restart a service
sudo systemctl enable --now nginx        # enable at boot and start now
journalctl -u nginx -n 100 --no-pager    # recent service logs
journalctl -f                             # follow system logs
```

`systemctl` and `journalctl` are common on systemd-based Linux distributions; they may not exist on every Unix system.

## 4. Permissions, users and environment

| Command | Use | Example |
|---|---|---|
| `whoami` | Current user | `whoami` |
| `id` | User/group IDs | `id` |
| `sudo` | Run a command with elevated privileges | `sudo systemctl status nginx` |
| `chmod` | Change permission bits | `chmod 755 script.sh`; `chmod u+x script.sh` |
| `chown` | Change owner/group | `sudo chown -R user:group app/` |
| `umask` | Default permission mask | `umask 022` |
| `env` / `printenv` | Show environment variables | `printenv PATH` |
| `export` | Set a variable for child processes | `export NODE_ENV=production` |
| `which` / `type` | Locate/explain a command | `type cd`; `which node` |
| `crontab` | Schedule recurring jobs | `crontab -e` |

Permission digits are calculated as `read=4`, `write=2`, `execute=1`:

```text
chmod 755 script.sh   # owner: rwx, group: r-x, others: r-x
chmod 640 secret.txt  # owner: rw-, group: r--, others: ---
```

Avoid `chmod -R 777` as a fix: it gives everyone write/execute access and creates a security risk. Give only the minimum permissions required.

## 5. Networking and remote work

| Command | Use | Example |
|---|---|---|
| `ip` | Inspect/configure interfaces and routes | `ip addr`; `ip route` |
| `ping` | Test basic reachability/latency | `ping -c 4 example.com` |
| `ss` | Inspect listening connections/ports | `ss -tulpn`; `ss -tan` |
| `curl` | Make HTTP/API requests | `curl -I https://example.com`; `curl -fsSL URL` |
| `wget` | Download a URL | `wget URL` |
| `dig` | Query DNS | `dig example.com`; `dig +short example.com` |
| `traceroute` | Show network path | `traceroute example.com` |
| `ssh` | Secure remote shell | `ssh user@server` |
| `scp` | Copy over SSH | `scp file user@server:/tmp/` |
| `rsync` | Efficient synchronisation | `rsync -avh --progress src/ server:/app/` |
| `nc` / `netcat` | Test TCP/UDP connections | `nc -vz localhost 8080` |

`curl -I` requests headers only. `curl -f` fails on HTTP errors, `-sS` is quiet but still shows errors, and `-L` follows redirects. These flags are useful in scripts.

## 6. Archives and reproducible transfers

```bash
tar -czf project.tgz project/       # create gzip-compressed archive
tar -xzf project.tgz                # extract it
tar -tzf project.tgz                # list contents without extracting
zip -r project.zip project/          # create ZIP
unzip project.zip                    # extract ZIP
sha256sum release.tar.gz             # calculate a checksum
```

Use checksums to verify that a downloaded or transferred file was not corrupted or replaced.

## 7. Git commands used by professional developers

```bash
git status                           # current branch and changes
git switch -c feature/login         # create and switch to a branch
git branch -vv                      # list branches and tracking info
git log --oneline --graph --decorate --all
                                     # compact visual history
git diff                             # unstaged changes
git diff --staged                    # staged changes
git add file.py                     # stage a file
git add -p                          # stage selected hunks interactively
git commit -m "Add login validation"
git restore file.py                 # discard unstaged changes in a file
git restore --staged file.py        # unstage without deleting edits
git fetch --prune origin            # update remote refs; does not modify your branch
git pull --rebase                   # fetch and replay local commits

git push -u origin feature/login    # publish branch and set upstream
git stash push -m "WIP"             # temporarily store uncommitted work
git stash pop                       # restore stashed work
git show <commit>                   # inspect a commit
git blame file.py                   # see who changed each line
git bisect start                    # find the commit that introduced a bug
git reflog                          # recover recently moved/lost branch tips
git cherry-pick <commit>            # apply one commit to current branch
```

### Professional Git habits

- Inspect `git diff` before staging and `git diff --staged` before committing.
- Prefer small, focused commits with meaningful messages.
- Use `git fetch` before deciding whether a branch needs merging/rebasing.
- Do not commit secrets; use `.gitignore` and secret management.
- `git reset --hard` and `git clean -fd` can destroy uncommitted work. Check `git status` first and make a backup when unsure.
- Rebase local unpublished work carefully; coordinate before rewriting a shared branch.

## 8. Common development commands

### Build and test

```bash
make                                # run the default target
make -j"$(nproc)"                  # parallel build on Linux
./configure && make && make test    # common C/C++ project pattern
```

### Python

```bash
python3 --version
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m pytest
python -m http.server 8000
```

Use `python -m pip` so pip belongs to the Python interpreter in use. Leave a virtual environment with `deactivate`.

### Node.js

```bash
node --version
npm install                         # install according to package-lock when present
npm ci                              # reproducible CI install from lockfile
npm run dev
npm test
npm run build
npx <tool>                          # run a project/local tool
```

Prefer the lockfile and the package manager selected by the project (`npm`, `pnpm` or `yarn`) rather than mixing them casually.

### Containers

```bash
docker build -t myapp:dev .
docker run --rm -p 8080:8080 myapp:dev
docker ps -a
docker logs -f <container>
docker exec -it <container> sh
docker compose up -d
docker compose logs -f service
docker compose down
```

`--rm` removes a stopped temporary container. Do not place passwords or API keys directly in shell history; use environment files/secrets appropriately.

### Kubernetes (common senior DevOps workflow)

```bash
kubectl get pods -A
kubectl get deployment myapp -n production
kubectl describe pod <pod> -n production
kubectl logs -f <pod> -n production
kubectl exec -it <pod> -n production -- sh
kubectl apply -f deployment.yaml
kubectl rollout status deployment/myapp -n production
kubectl rollout undo deployment/myapp -n production
```

Always verify the current context (`kubectl config current-context`) before applying or deleting resources, especially in production.

## 9. Debugging and performance tools

- `strace -f -p PID` (Linux): observe system calls made by a process.
- `lsof -p PID`: see files and sockets opened by a process.
- `gdb ./program`: debug native programs; use breakpoints, `run`, `bt` and `print`.
- `valgrind --leak-check=full ./program`: find many memory errors/leaks in native programs.
- `curl -v URL`: inspect HTTP request/response details.
- `openssl s_client -connect host:443 -servername host`: inspect a TLS connection.
- `shellcheck script.sh`: find common shell-script mistakes.
- `git bisect`: binary-search history for the commit introducing a regression.

Useful shell-script starting point:

```bash
#!/usr/bin/env bash
set -Eeuo pipefail
```

This makes many mistakes visible (`-e` stops on errors, `-u` catches unset variables, `pipefail` propagates pipeline failures). Learn its edge cases before using it blindly in complex scripts.

## 10. Windows PowerShell equivalents

If you use Windows rather than Linux/WSL, these are useful equivalents:

| Linux/Bash | PowerShell | Purpose |
|---|---|---|
| `pwd` | `Get-Location` / `pwd` | Current directory |
| `ls` | `Get-ChildItem` / `ls` | List files |
| `cd` | `Set-Location` / `cd` | Change directory |
| `cp` | `Copy-Item` / `cp` | Copy |
| `mv` | `Move-Item` / `mv` | Move/rename |
| `rm` | `Remove-Item` / `del` | Delete |
| `cat` | `Get-Content` / `cat` | Read file |
| `grep` | `Select-String` | Search text |
| `find` | `Get-ChildItem -Recurse` | Search files |
| `ps` | `Get-Process` | List processes |
| `kill PID` | `Stop-Process -Id PID` | Stop process |
| `df` | `Get-PSDrive` | Drive space |
| `env` | `Get-ChildItem Env:` | Environment |
| `curl` | `Invoke-WebRequest` (or installed `curl.exe`) | HTTP request |
| `ip addr` | `Get-NetIPConfiguration` | Network configuration |
| `ss` | `Get-NetTCPConnection` | TCP connections |
| `chmod` | `icacls` | Permissions |
| `ssh` | `ssh` | Remote shell |

PowerShell pipelines pass objects rather than only text. For example:

```powershell
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
Get-ChildItem -Recurse -File | Select-String "TODO"
```

## 11. Senior-developer command-line habits

1. **Read help first:** `command --help`, `man command`, or official documentation.
2. **Inspect before modifying:** use `git diff`, `ls`, `find`, `ps`, `kubectl config current-context`, etc.
3. **Use least privilege:** do not use `sudo` unless necessary.
4. **Prefer dry runs:** use flags such as `rsync --dry-run`, package-manager preview options and Git diff.
5. **Quote variables:** use `"$file"`, especially in scripts, to handle spaces safely.
6. **Make commands reproducible:** record versions, use lockfiles, scripts and environment files rather than undocumented manual steps.
7. **Log long operations:** `command 2>&1 | tee run.log`.
8. **Use exit codes:** chain commands with `&&`, check `$?`, and fail clearly in automation.
9. **Avoid secrets in arguments:** command-line arguments can appear in history/process listings; use secret stores or protected environment mechanisms.
10. **Know destructive commands:** pause before `rm -rf`, `kill -9`, `chmod -R`, `git reset --hard`, `git clean -fd`, `docker system prune` or `kubectl delete`.

### Module connection

These commands are also practical demonstrations of the syllabus: `ps/top` show processes and scheduling; `free/vmstat` show memory and paging; `iostat/df` show storage and I/O; `ss/lsof` show communication and open resources; `systemctl/journalctl` show OS services; and `chmod/chown` demonstrate protection and security.

# Exam-answer checklist

Before submitting an answer, check:

- Did I define the term in the first sentence?
- Did I draw a labelled diagram/Gantt chart/table when appropriate?
- For an algorithm, did I state its steps and assumptions?
- For a numerical answer, did I show intermediate work, not only the final value?
- Did I mention one advantage and one limitation for comparisons?
- Did I distinguish similar terms (waiting vs response, deadlock vs starvation, paging vs segmentation)?
- Did I use consistent units and clearly state tie-breaking rules, disk direction and frame count?

**Best final revision order:** scheduling numericals → semaphores/classical problems → Banker’s algorithm → page replacement/TLB → disk scheduling → file allocation → definitions and comparisons.
