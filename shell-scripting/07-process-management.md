# Process Management in Shell Scripting

## 1. What is a Process?

A process is an instance of a running program. Every process has:

- A unique Process ID (PID).
- An associated parent process (PPID).
- A state (running, sleeping, zombie, etc.).

### Why is Process Management Important?

Process management allows system administrators and developers to:

1. Monitor running processes.
2. Start, stop, and resume processes.
3. Manage resources efficiently (CPU, memory).
4. Automate tasks using scripts.

## 2. Basic Commands for Process Management

| Command | Description |
|---|---|
| `ps` | Display information about active processes |
| `top` | Show real-time process information |
| `pgrep` | Search for processes by name |
| `kill` | Terminate a process using its PID |
| `jobs` | Show background and stopped jobs |
| `fg` | Bring a background process to the foreground |
| `bg` | Resume a stopped process in the background |
| `nice` / `renice` | Adjust process priority |
| `nohup` | Run a command immune to hangups (disconnection) |

## 3. Viewing Processes

### 3.1 The `ps` Command

The `ps` command displays information about currently running processes.

**Common options:**

- `ps aux`: Show all processes with details like PID, CPU, memory usage, and command.
- `ps -eo pid,user,command`: Display specific fields for all processes.

```bash
ps aux
```

### 3.2 The `top` Command

The `top` command displays processes in real time, sorted by CPU usage.

```bash
top
```

**Key interactions:**

- Press `q` to quit.
- Press `k` and enter a PID to kill a process.
- Press `r` and enter a PID to renice (adjust priority) a process.

## 4. Searching for Processes

### 4.1 The `pgrep` Command

The `pgrep` command searches for processes by name.

```bash
pgrep bash
```

This outputs the PID of every process named `bash`.

### 4.2 Searching with `grep`

You can pipe `ps` output through `grep` to search for processes.

```bash
ps aux | grep "process_name" | grep -v grep
```

`grep -v grep` excludes the `grep` command itself from the results.

## 5. Starting and Stopping Processes

### 5.1 Starting a Process in the Background

To run a process in the background, add `&` at the end of the command.

```bash
ping google.com > output.txt &
```

### 5.2 Viewing Background Processes

Use the `jobs` command to view background jobs.

```bash
jobs
```

### 5.3 Bringing Processes to Foreground or Background

**Foreground** — use `fg` with the job ID:

```bash
fg %1
```

**Background** — use `bg` to resume a stopped process in the background:

```bash
bg %1
```

### 5.4 Killing a Process

Terminate a process using the `kill` command with its PID.

**Syntax:**

```bash
kill <PID>
```

Example:

```bash
kill 12345
```

### 5.5 Forcefully Killing a Process

Use `kill -9` to forcefully terminate a process.

```bash
kill -9 12345
```

## 6. Process States

| State | Description |
|---|---|
| `R` | Running or runnable |
| `S` | Sleeping (waiting for an event) |
| `D` | Uninterruptible sleep (I/O wait) |
| `Z` | Zombie (terminated but not cleaned up) |
| `T` | Stopped (suspended) |

## 7. Advanced Process Management

### 7.1 Adjusting Process Priority with `nice` and `renice`

- `nice`: Start a process with a specific priority.
- `renice`: Change the priority of an already-running process.

**Syntax:**

```bash
nice -n 10 command
renice 5 -p <PID>
```

### 7.2 Monitoring Resource Usage

Use `top` or `ps` to monitor CPU and memory usage.

**Example — top 5 CPU-consuming processes:**

```bash
ps -eo pid,%cpu,cmd --sort=-%cpu | head -n 6
```

### 7.3 Detecting Zombie Processes

Zombie processes have already completed execution but haven't been cleaned up by their parent process.

```bash
ps aux | awk '$8 ~ /Z/ {print $2, $11}'
```

## 8. Practical Examples

### Example 1: Start and Monitor a Background Process

```bash
#!/bin/bash
ping google.com > ping_output.txt &
echo "Ping started in the background. PID: $!"
```

### Example 2: Monitor Top CPU-Consuming Processes

```bash
#!/bin/bash
echo "Top 5 CPU-consuming processes:"
ps -eo pid,%cpu,%mem,cmd --sort=-%cpu | head -n 6
```

### Example 3: Kill a Process by Name

```bash
#!/bin/bash
read -p "Enter the process name to kill: " process_name
pid=$(pgrep -x "$process_name")

if [[ -n "$pid" ]]; then
    kill "$pid"
    echo "Process '$process_name' (PID: $pid) terminated."
else
    echo "Process '$process_name' not found."
fi
```

### Example 4: Schedule a Task After a Delay

```bash
#!/bin/bash
read -p "Enter the command to run: " command
read -p "Enter the delay time (seconds): " delay

echo "Task will run after $delay seconds..."
sleep "$delay"
eval "$command"
echo "Task executed."
```

### Example 5: Detect and Log Zombie Processes

```bash
#!/bin/bash
zombies=$(ps aux | awk '$8 ~ /Z/ {print $2, $11}')

if [[ -z "$zombies" ]]; then
    echo "No zombie processes detected."
else
    echo "Zombie processes found:"
    echo "$zombies" | tee zombie_log.txt
fi
```

## 9. Summary

| Command | Purpose |
|---|---|
| `ps` | Display running processes |
| `top` | Real-time process monitoring |
| `kill` | Terminate processes by PID |
| `pgrep` | Find processes by name |
| `jobs` | Display background jobs |
| `fg` / `bg` | Foreground or background job control |
| `nice` / `renice` | Adjust process priorities |
| `sleep` | Delay execution of commands |
