# Common Linux Commands for Kali Linux

Quick reference for the commands that come up constantly when working in a Kali environment, grouped by task.

## 1. File and Directory Management

| Command | Description |
|---|---|
| `ls` | List files and directories |
| `cd` | Change directory |
| `pwd` | Print working directory |
| `mkdir` | Create a new directory |
| `rmdir` | Remove an empty directory |
| `cp` | Copy files or directories |
| `mv` | Move or rename files or directories |
| `rm` | Remove files or directories |

## 2. File Viewing and Editing

| Command | Description |
|---|---|
| `cat` | View file contents |
| `nano`, `vim` | Text editors |
| `less` | View file contents one page at a time |
| `head` | View the first few lines of a file |
| `tail` | View the last few lines of a file |

## 3. File Permissions

| Command | Description |
|---|---|
| `chmod` | Change file permissions |
| `chown` | Change file owner |

## 4. Process Management

| Command | Description |
|---|---|
| `ps` | Display currently running processes |
| `top` | Display system resource usage |
| `kill` | Terminate a process |
| `htop` | Interactive process viewer (may need to be installed) |

## 5. Networking

| Command | Description |
|---|---|
| `ifconfig` | Display or configure network interfaces |
| `ping` | Test network connectivity |
| `netstat` | Network statistics |
| `curl` | Transfer data from or to a server |

## 6. Package Management

| Command | Description |
|---|---|
| `apt-get` | Install, update, or remove packages |
| `dpkg` | Install or remove packages manually |

## 7. System Monitoring

| Command | Description |
|---|---|
| `df` | Report disk space usage |
| `du` | Estimate file space usage |
| `free` | Display memory usage |

## 8. User Management

| Command | Description |
|---|---|
| `adduser` | Add a new user |
| `passwd` | Change a user's password |
| `su` | Switch user |
| `sudo` | Execute a command as another user, typically root |

## 9. File Searching

| Command | Description |
|---|---|
| `find` | Search for files and directories |
| `grep` | Search text using patterns |

## Final Tips

- Always start your scripts with `#!/bin/bash`.
- Use `chmod +x` to make your scripts executable.
- Test your scripts with simple commands before adding complexity.
- Use `man` followed by a command (e.g., `man ls`) to see the manual and options for that command.
