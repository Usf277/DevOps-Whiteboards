# Managing Linux Processes

## What is a process in Linux?

A process is a running instance of a command. It's a file that has been executed, read from the disk, loaded into memory, and executed.

## What is actually being executed?

- A C program
- A shell script
- A command in the terminal
- Commands (external & built-in commands in the shell itself)

## The Origin of All Processes in Linux

All processes in Linux start from a single process called **systemd** (or **sysVinit** in older distributions). This is the **parent process** that manages all other processes.

- `systemd` takes **PID 1**
- Every process has a **parent process**
- Even `PID 1` has a parent process, which is the **kernel** itself (PID 0)

## Types of Processes

### System Process

- Operates when the Linux OS boots up
- Managed by the kernel

### User Process

- Started by the user manually, remotely, or via scheduling

### Services (Daemons)

- Background processes that run independently of user interaction
- Provide specific functionality to the system or applications

---

## Managing Linux Processes

Managing processes in Linux is a fundamental aspect of system administration. Process management involves **creating, monitoring, controlling, and terminating** processes.

### Process Lifecycle in Linux

A process goes through multiple stages in its lifetime:

1. **Create (Forking)**
   - A new process is created using `fork()`
   - Some processes are created via **exec()** to run a new program

2. **Ready (Runnable State)**
   - The process is ready but waiting for the CPU
   - Managed by the **scheduler**

3. **Running (Executing)**
   - The process is actively executing instructions
   - The OS gives it CPU time

4. **Terminated (Killed or Exit)**
   - A process finishes execution and exits
   - Can also be forcefully **killed**

5. **Stopped (Suspended/Traced)**
   - A paused process (e.g., via `Ctrl+Z`)
   - Can be resumed with `fg` or `bg`

---

## Commands/Topics Related to Managing Linux Processes

### Listing Processes Commands

- `ps aux` → Lists all active processes
- `top` → Dynamic real-time view of running processes
- `htop` → Enhanced version of `top`
- `pgrep <process-name>` → Searches for a process by name

### Killing Processes Commands

- `kill <PID>` → Sends a termination signal to a process
- `killall <process-name>` → Kills all processes with a specific name
- `pkill <pattern>` → Kills processes matching a pattern
- `xkill` → Kills a process by clicking on its window

#### Types of Signals in Linux

| Signal | Name  | Description |
|--------|--------|----------------|
| `1` | SIGHUP | Hangup |
| `9` | SIGKILL | Force kill |
| `15` | SIGTERM | Graceful termination |
| `19` | SIGSTOP | Stop a process |
| `18` | SIGCONT | Resume a process |

### Hiding and Freezing Processes

- `nohup <command> &` → Runs a command in the background
- `jobs` → Lists background jobs
- `fg <job-number>` → Brings a job to the foreground
- `bg <job-number>` → Resumes a stopped job in the background
- `disown -h <job-number>` → Detaches a job from the terminal

---

## Background and Foreground Processes

### Running a program in the background

- Append `&` at the end of the command:

  ```sh
  command &
  ```

- View background jobs with `jobs`

### Bringing a background process to the foreground

- Use `fg` to resume execution:

  ```sh
  fg %1
  ```

- Suspend a process with `Ctrl+Z`

---

## Process States in Linux

Every process in Linux exists in different states:

1. **Running** (`R`): Actively using the CPU
2. **Sleeping** (`S`): Waiting for an event
3. **Zombie** (`Z`): Finished execution but not removed from the process table
4. **Stopped** (`T`): Paused or debugging

---

## The Concept of cgroups

Cgroups (Control Groups) limit and manage system resources for processes, users, or applications.

### Examples of Resource Control

- **CPU Limits**: Restrict CPU usage per process
- **Memory Limits**: Prevent processes from exceeding memory allocation
- **Network Limits**: Control network bandwidth usage

### How cgroups Work

Cgroups organize processes into hierarchical groups, allowing fine-grained resource control.

---

## Example Use Case in Action

Cloud providers like AWS and Google Cloud use cgroups to manage multiple virtual machines efficiently. It ensures:

- Fair resource allocation
- Prevents a single process from monopolizing CPU, memory, or disk I/O
