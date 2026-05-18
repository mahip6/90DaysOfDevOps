# Linux Study Notes

---

## Core Components

### The Kernel
- The **kernel** is the heart of Linux — it sits between hardware and software.
- Manages CPU scheduling, memory allocation, device drivers, and system calls.
- Runs in **kernel space** (privileged mode) — direct hardware access, no safety net.
- Key subsystems: process scheduler, virtual memory manager, VFS (Virtual File System), networking stack.

### User Space
- Everything else runs here — your shell, apps, daemons, libraries.
- Communicates with the kernel via **system calls** (`read()`, `write()`, `fork()`, `exec()`).
- Protected from each other and the kernel — a crash here doesn't take down the system.
- The C standard library (`glibc`) wraps system calls into friendly functions.

### init / systemd
- **PID 1** — the first process the kernel starts after boot.
- All other processes are descendants of PID 1.
- Modern systems use **systemd** as init (replaces older SysV init and Upstart).
- Responsible for bringing the system from power-on to a usable state.

---

## How Processes Are Created & Managed

### The fork-exec Model
- **`fork()`** — duplicates the current process; creates a child with a new PID.
- **`exec()`** — replaces the child's memory with a new program image.
- **`wait()`** — parent waits for child to finish and collects its exit status.
- Every process has: a PID, a PPID (parent PID), UID/GID, file descriptors, and a memory map.

### Process States

| State | Symbol | Meaning |
|---|---|---|
| **Running** | `R` | On CPU right now, or in the run queue ready to go |
| **Sleeping (interruptible)** | `S` | Waiting for an event (I/O, signal) — can be woken by a signal |
| **Sleeping (uninterruptible)** | `D` | Deep kernel sleep, usually waiting on disk I/O — cannot be killed |
| **Stopped** | `T` | Paused by `SIGSTOP` or a debugger (`Ctrl+Z`) |
| **Zombie** | `Z` | Process finished, but parent hasn't called `wait()` yet — entry stays in the process table |
| **Idle** | `I` | Kernel thread with nothing to do |

> **Zombie tip:** A zombie can't be killed — it's already dead. Kill the parent, or wait for it to call `wait()`.

---

## What systemd Does (and Why It Matters)

### Core Responsibilities
- **Parallel startup** — launches services concurrently instead of sequentially (faster boot).
- **Dependency management** — declares `Before=`, `After=`, `Requires=` between units.
- **Process supervision** — automatically restarts crashed services (`Restart=on-failure`).
- **Logging** — collects logs from all services into the **journal** (`journalctl`).
- **Targets** — replaces old runlevels; e.g., `multi-user.target` = text login, `graphical.target` = GUI.

### Key systemd Concepts
- **Unit files** — config files in `/etc/systemd/system/` or `/lib/systemd/system/` (`.service`, `.socket`, `.timer`, `.mount`).
- **Cgroups integration** — every service gets its own cgroup; systemd tracks and limits resource usage.
- **Socket activation** — starts a service only when its socket receives a connection (lazy loading).

### Example: Checking a Service
```bash
systemctl status nginx          # Is it running?
systemctl restart nginx         # Restart it
systemctl enable nginx          # Start on boot
journalctl -u nginx -f          # Tail its logs live
```

---

## 5 Commands You'll Use Daily

### 1. `ps aux` — Snapshot of running processes
```bash
ps aux                          # All processes, all users
ps aux | grep nginx             # Find a specific process
```
Columns to know: `PID`, `%CPU`, `%MEM`, `STAT` (process state), `COMMAND`.

### 2. `top` / `htop` — Live process monitor
```bash
top                             # Built-in, always available
htop                            # Friendlier, colour-coded (may need install)
```
Press `k` in top to kill a process by PID. `htop` lets you scroll and click.

### 3. `systemctl` — Manage services
```bash
systemctl status sshd           # Check SSH daemon status
systemctl list-units --failed   # See what's broken at a glance
```
Your single tool for start / stop / restart / enable / disable any service.

### 4. `journalctl` — Read system logs
```bash
journalctl -xe                  # Last logs + explanations (great for debugging)
journalctl -u cron --since "1h ago"   # Logs for one service, last hour
journalctl -b                   # Logs from current boot only
```

### 5. `strace` — Trace system calls in real time
```bash
strace ls                       # See every syscall ls makes
strace -p 1234                  # Attach to a running process by PID
```
Invaluable for debugging "why is this process hanging?" or "what file is it trying to open?".

---

## Quick Mental Model

```
Hardware
   ↑
Kernel (scheduler, memory, drivers)   ← kernel space
   ↑  (system calls)
glibc / standard libraries
   ↑
User processes (shell, apps, daemons) ← user space
   ↑
systemd (PID 1) orchestrates it all from boot
```

---
