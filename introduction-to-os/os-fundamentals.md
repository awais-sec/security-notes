# OS Fundamentals

## What an Operating System Is

An operating system is a program that acts as an intermediary between a user of a computer and the computer hardware, providing the user a simpler (virtual) machine to work with. It's also a resource manager: a program that allocates and deallocates computer system resources in an efficient, fair, and secure manner.

## Types of OS

### Real-time operating systems

Designed to meet well-defined, fixed-time constraints. Commonly used where timing is crucial: controlling scientific experiments, medical imaging systems, industrial control systems, plane landings, ventilators.

- **Hard real-time systems**: Strict constraints, no secondary storage limitations issue since data is stored in short-term memory or ROM, and there's no virtual memory (to avoid wasting time on address translation). Examples: plane landing systems, process control in nuclear power plants, ventilators.
- **Soft real-time systems**: Time constraints exist for producing output, but missing a deadline isn't life-threatening. Examples: live video streaming, mobile communication, music-playing robots, weather stations.

### Time-sharing operating systems

Multi-user, multi-process systems that let multiple users interact with the system simultaneously. Use multiprogramming to swap jobs in and out of main memory to achieve reasonable response times. Examples: UNIX, Linux, Windows NT Server, Windows 2000 Server.

### Multi-programmed operating systems

Keep several jobs in main memory simultaneously and multiplex the CPU among them, improving CPU utilization and throughput. Example: two processes, each with CPU and I/O bursts of one time unit each, running in an interleaved fashion.

**Summary**: real-time OS focuses on meeting strict time constraints, time-sharing OS allows multiple simultaneous users, and multi-programmed OS keeps multiple jobs in memory to improve CPU utilization. Each serves a different purpose depending on the requirements of the applications it supports.

## Components of an Operating System

1. Process management
2. Main memory management
3. Secondary storage management
4. I/O system management
5. File management
6. Protection system
7. Networking
8. Command-line interpreter (shells)

### Process management

Manages how a process starts and ends, providing the mechanisms for that. More formally: creating and terminating both user and system processes, suspending and resuming processes, providing mechanisms for process synchronization and communication, and handling deadlock situations.

### Main memory management

Tracks free memory space, monitors memory usage by processes, decides which processes to load into memory, allocates and deallocates memory space as needed, and ensures processes don't overwrite each other in memory.

## Process State

The process state refers to the various stages a process goes through during execution. As a process executes, it changes state:

- **new**: The process is being created
- **running**: Instructions are being executed
- **waiting**: The process is waiting for some event to occur
- **ready**: The process is waiting to be assigned to a processor
- **terminated**: The process has finished execution
