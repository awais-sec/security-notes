# Linux Forensics and File Systems

## Linux File System Architecture

The Linux file system architecture is divided into two spaces: User Space and Kernel Space.

- User applications interact with the file system via the GNU C Library and a system call interface.
- Kernel Space includes components such as the inode cache, virtual file system, individual file systems, buffer cache, and device drivers.

## Filesystem Hierarchy Standard (FHS)

The FHS defines the directory structure and its contents in Linux and Unix-like operating systems. All files and directories exist under the root directory (`/`).

Key directories:

| Directory | Purpose |
|---|---|
| `/bin` | Essential command binaries (`ls`, `cat`, `cp`) |
| `/boot` | Static boot loader files (kernels) |
| `/dev` | Essential device files (`/dev/null`) |
| `/etc` | Host-specific system configuration files |
| `/home` | Users' home directories |
| `/lib` | Essential libraries for binaries |
| `/media` | Mount points for removable media |
| `/mnt` | Temporarily mounted file systems |
| `/opt` | Add-on application software packages |
| `/root` | Home directory for the root user |
| `/proc` | Virtual file system providing process and kernel information as files |
| `/run` | Information about running processes |
| `/sbin` | Binary files required for system operation |
| `/srv` | Site-specific data for services provided by the system |
| `/tmp` | Temporary files |
| `/usr` | Secondary hierarchy for read-only user data |
| `/var` | Variable data (logs, spool files) |

## Extended File Systems (EXT)

- **EXT**: The first Linux file system, designed to overcome Minix file system limitations. Max partition size 2GB, max filename 255 characters. Replaced by EXT2.
- **EXT2**: A standard file system with improved algorithms and additional timestamps. Maintains a superblock field tracking whether the file system is clean or dirty. Shortcomings: risk of corruption when writing, and it's not a journaling file system.
  - **Inodes**: The basic building block of EXT2. Each file and directory is described by a single inode, and inodes are placed together in an inode table.
  - **Directories**: Files that hold the access path of files, containing directory entries with the directory inode, filename length, and directory name.
- **EXT3**: The journaling version of EXT2, widely used in Linux. Provides stronger data integrity around system shutdowns, higher throughput than EXT2, and allows easy migration from EXT2. Journaling records file system updates for quick recovery after a crash; the journal uses inode 8, with its location stated in the superblock. The first journal block holds the superblock and general information.
- **EXT4**: A journaling file system replacing EXT3, with significant improvements in performance, scalability, and reliability. Supports Linux Kernel v2.6.19 onward. Key features: max individual file size 16TB, max file system size 1EB, extents replacing EXT2/EXT3 block mapping, delayed allocation, multi-block allocation, faster file system checking, journal checksumming, persistent preallocation, improved timestamps, and backward compatibility.

## Linux Forensics

### Shell commands for investigation

| Command | Purpose |
|---|---|
| `dmesg` | Displays kernel ring buffers / device driver information |
| `fsck` | File system consistency check, and repair |
| `stat` | Displays file or file system status |
| `history` | Lists the Bash log of typed commands |
| `mount` | Mounts a file system |
| `ps` | Reports status of current processes |
| `pstree` | Displays processes as a tree |
| `grep` | Searches for text or expressions in files |
| `top` | System summary information and process list |
| `kill` | Terminates processes |
| `file` | Displays the type of data contained in a file |
| `su` | Runs a command with a substitute user/group ID |
| `dd` | Copies, converts, and formats a file |
| `ls` | Lists directory contents |
| `pgrep` | Process-ID Global Regular Expressions Print, searches current processes and lists matching PIDs |

### Linux log files

| Path | Contents |
|---|---|
| `/var/log/auth.log` | System authorization info, logins, authentication |
| `/var/log/kern.log` | Kernel initialization, errors, informational messages |
| `/var/log/faillog` | Failed user login attempts |
| `/var/log/lpr.log` | Printer logs |
| `/var/log/mail.*` | Mail server messages |
| `/var/log/mysql.*` | MySQL server logs |
| `/var/log/apache2/*` | Apache web server logs |
| `/var/log/apport.log` | Application crash reports |
| `/var/log/lighttpd/*` | Lighttpd web server logs |
| `/var/log/daemon.log` | Running services (squid, ntpd, etc.) |
| `/var/log/debug` | Debugging log messages |
| `/var/log/dpkg.log` | Package installation/removal logs |

### Collecting volatile data

Volatile data is temporary and lost when a system powers off.

| Command | Purpose |
|---|---|
| `netstat` | Network info: routing tables, connections, interface stats |
| `last -F` | Full login/logout times and dates |
| `hostname` | System hostname |
| `ifconfig -a` | Configuration of all network interfaces |
| `lsof` | Open files and the processes that opened them |
| `lsmod` | Loaded kernel modules |
| `xclip -o` | Outputs clipboard contents (X clipboard CLI) |
| `aureport` | Summary report of audit daemon logs |
| `id [username]` | User ID for the specified username |
| `ausearch [userid]` | Tracks user events for a particular user ID |
| `arp` | Extracts the ARP cache |
| `ss -l -p -n \| grep <PID>` | Checks whether a specific process is suspicious |
| `cat /proc/version` | Linux kernel version |
| `cat /proc/sys/kernel/domainname` | Domain name |
| `cat /proc/swaps` | Total and used swap space |
| `cat /proc/partitions` | Disk partitions |
| `cat /proc/cpuinfo` | CPU details |
| `cat /proc/mounts` (or `/proc/self/mounts`) | Mounted file systems |
| `cat /proc/uptime` | System uptime |

Other volatile sources: `/var/spool/cron/` and `/etc/cron.daily` (scheduled tasks), `.bash_history` (command history), `/proc` directory (current system state), ELF (Executable and Linking Format, the main Linux executable format).

### Collecting non-volatile data

Non-volatile data persists after the system is powered down.

- `smbtree` / `smbclient -L localhost`: Shows connections and shared files.
- `ls /etc/rc*.d` (or `/etc/rc.d`): Checks for auto-start services.
- `find /etc -type f -print | xargs stat | sort -r` (or the `-exec stat -c %Y:%i %n {} \; | sort -nr` variant): Lists recently modified files.
- Log files in `/var/log` (e.g. `cat /var/log/auth.log`): Login and system logs.
- `ls -la /dev`: Search for files with strange names.
- `chkrootkit`: Checks security settings for anomalies.
- `ls -l` / `fls -d`: Finds deleted files and associated data.
- Linux Volume Manager (LVM) / `parted`: Detects unallocated partitions and files.

### Swap space

Swap space is storage on a hard disk used as a virtual memory extension of RAM.

- If RAM is 2GB or less, swap space should be twice the physical RAM.
- If RAM is more than 2GB, swap space should be 2GB more than the physical RAM.
- `swapon -s` shows current swap space information.
