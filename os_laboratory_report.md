# Laboratory Report 1: Process Creation, Process States, Process Control Block, and Context Switching

**Course & Section:** BCA151 Operating Systems Lab  
**Student Name:** Miche Louie B. Acompañado
**Date:** October 22, 2026  
**Environment:** GitHub Codespaces (Linux Cloud Container)  

---

## 1. Introduction

A process is an active instance of a computer program being executed by the operating system kernel. When a user or system initiates an executable file, the OS creates a new process, assigns it a unique Process ID (PID), allocates virtual memory pages, and initializes a dedicated **Process Control Block (PCB)** data structure to track its execution state, resource consumption, and CPU scheduling parameters.

This laboratory activity explores process creation, state analysis, real-time monitoring, and multi-instance concurrency using standard Linux utilities (`ps`, `grep`, and `top`) within GitHub Codespaces.

---

## 2. Learning Outcomes

Upon completing this activity, the following capabilities were demonstrated:
1. Created and managed an OS laboratory repository (`OS-Laboratory-1`) on GitHub and GitHub Codespaces.
2. Implemented and executed a Python process control script (`process_lab.py`).
3. Identified and recorded PIDs assigned by the Linux kernel during execution.
4. Monitored active processes and resource metrics using `ps aux` and interactive `top`.
5. Interpreted Linux process state codes (`R`, `S`, `+`).
6. Correlated CLI outputs directly to internal Process Control Block (PCB) fields.
7. Analyzed concurrent process execution, CPU time slicing, and context switching.

---

## 3. Required Components

* GitHub Account & Repository (`OS-Laboratory-1`)
* GitHub Codespaces Cloud Development Environment
* Linux Bash Terminal (`Ubuntu/Debian` container environment)
* Python 3 Interpreter (`python3`)
* Web Browser & Internet Access

---

## 4. Procedure, Observations, and Data Collection

### Part A & B – Repository and Codespace Initialization
* Created a private repository named `OS-Laboratory-1` with a `README.md` file.
* Launched the development container via **GitHub Codespaces**.

### Part C & D – Program Implementation
* Created `process_lab.py` to retrieve its PID using `os.getpid()` and execute a timed 15-iteration delay loop (`time.sleep(2)`) to provide a sustained window for system monitoring.

---

### Part E – Executing the Process

* **Step 8: Python Version Check**
  ```bash
  python --version
  ```
  **Recorded Python Version:** `Python 3.10.12`

* **Step 9: Running the Program**
  ```bash
  python process_lab.py
  ```
  *Sample Program Output:*
  ```text
  ==========================================
       OPERATING SYSTEMS LABORATORY
       PROCESS MONITORING ACTIVITY
  ==========================================
  Process ID (PID): 27778
  Process has been created.
  Process is executing...

  Execution step: 1
  Execution step: 2
  ...
  ```

---

### Part F – Process Identification

* **Step 10: Recorded PID:** `27778`

* **Discussion Question:** *Why does the operating system assign a PID to every process?*  
  **Answer:**  
  The operating system kernel assigns a unique Process ID (PID) to every process to uniquely identify and track it among all active system processes. The PID serves as a key handle for the kernel to allocate virtual memory, track CPU execution time, enforce security/user permissions, manage I/O streams, and deliver system signals (such as `SIGKILL` or `SIGSTOP`).

---

### Part G – Process Monitoring with `ps` and `ps aux`

* **Step 11 & 12: Executing `ps aux`**
  ```bash
  ps aux
  ```

#### Recorded Metrics for `process_lab.py`

| Information Field | Observed Value | Description |
| :--- | :--- | :--- |
| **PID** | `27778` | Unique process ID assigned by the kernel. |
| **CPU %** | `0.0%` | Percentage of total CPU time used during execution. |
| **Memory %** | `0.1%` | Percentage of total physical RAM consumed. |
| **User** | `codespace` | System user account owning the process. |
| **Command** | `python process_lab.py` | Full executable path and command arguments. |

---

### Part H – Filtering Processes with `grep`

* **Step 13: Executing `ps aux | grep process_lab`**
  ```bash
  ps aux | grep process_lab
  ```

* **Discussion Question:** *How does the `grep` command help us locate a specific process?*  
  **Answer:**  
  The `ps aux` command outputs every running process in the system, which can amount to hundreds of lines. By piping (`|`) the output of `ps aux` into `grep process_lab`, `grep` acts as a text filter that scans the stream line-by-line and returns only the entries containing the string `"process_lab"`. This allows administrators and users to instantly locate specific programs without manually searching through large process lists.

---

### Part I – Observing Process States

* **Step 14: Executing `ps -o pid,stat,cmd`**
  ```bash
  ps -o pid,stat,cmd
  ```

#### Observed State Code Output
```text
  PID STAT CMD
27778 S+   python process_lab.py
```

#### Linux Process State Reference Table

| Code | Meaning | Context / Description in Lab |
| :---: | :--- | :--- |
| **R** | Running / Runnable | Process is actively executing on the CPU or in the run queue. |
| **S** | Interruptible Sleep | Process is waiting for an event (e.g., `time.sleep(2)` or I/O completion). |
| **D** | Uninterruptible Sleep | Process is waiting for disk/hardware I/O and cannot be interrupted by signals. |
| **T** | Stopped | Process has been suspended by a job control signal (e.g., `Ctrl + Z`). |
| **Z** | Zombie | Process has completed execution but its parent has not yet read its exit status. |
| **+** | Foreground Group | Added to state code when the process is running in the foreground terminal. |

---

### Part J – Resource Monitoring with `top`

* **Step 15: Executing `top`**
  ```bash
  top
  ```

#### Observed Real-Time Metrics (`python` process)

| Metric | Observed Value in `top` |
| :--- | :--- |
| **PID** | `36010` |
| **CPU Utilization (`%CPU`)** | `0.0%` |
| **Memory Utilization (`%MEM`)** | `0.1%` |
| **Process State (`S`)** | `S` (Interruptible Sleep) |
| **Process Name (`COMMAND`)** | `python` |

---

### Part K – Relating the Activity to the Process Control Block (PCB)

The operating system kernel maintains process state using an internal memory structure called the **Process Control Block (PCB)**.

```text
             PROCESS
                │
                ▼
       ┌─────────────────┐
       │       PCB       │
       ├─────────────────┤
       │ Process ID      │
       │ Process State   │
       │ CPU Context     │
       │ Scheduling Info │
       │ Memory Info     │
       │ I/O Information │
       └─────────────────┘
```

#### Mapping Terminal Observations to PCB Attributes

| Observed Terminal Field | Corresponding PCB Field | Operating System Functionality |
| :--- | :--- | :--- |
| `PID` | **Process ID** | Unique integer key used by the kernel to reference the process. |
| `STAT` / `S` (`S+`, `R`) | **Process State** | Tracks current lifecycle state (`Running`, `Sleeping`, `Ready`, `Blocked`). |
| `%CPU`, `TIME+` | **CPU Context** | Saved CPU register state (Program Counter, Stack Pointer) and elapsed execution time. |
| `PR`, `NI` | **Scheduling Info** | Base priority and nice value used by the OS scheduler to determine CPU timeslices. |
| `%MEM`, `VIRT`, `RES` | **Memory Info** | Pointers to page tables, virtual memory size, and allocated physical memory frame counts. |
| `TTY` (`pts/0`, `pts/1`) | **I/O Information** | Allocated input/output terminal streams, active file descriptors, and socket connections. |

* **Discussion Question:** *Which information observed using `ps` and `top` could be associated with information maintained by the operating system for a process?*  
  **Answer:**  
  All metrics displayed by `ps` and `top` reflect live data retrieved directly from the PCB maintained in kernel memory:
  1. **Process Identifier:** `PID` and `USER` match the PCB's Process Identification and Credentials fields.
  2. **Process State:** `STAT` / `S` reflects the current scheduling state tracked in the PCB.
  3. **CPU Context & Execution Stats:** `%CPU` and `TIME+` reflect the accumulated CPU time and saved execution context.
  4. **Scheduling Parameters:** `PR` (Priority) and `NI` (Nice Value) map directly to PCB Scheduling Information.
  5. **Memory Allocation:** `VIRT`, `RES`, and `%MEM` represent the virtual memory mappings and page tables linked in the PCB.
  6. **I/O Allocation:** `TTY` represents the active controlling terminal handles stored in the PCB's I/O section.

---

### Part L – Multi-Process Concurrency Observation

* **Step 16: Simultaneous Program Execution**  
  Opened three distinct terminal tabs and executed `python process_lab.py` concurrently in each.

* **Step 17: Multi-Process Inspection Output**
  Executed in Terminal 4:
  ```bash
  ps aux | grep process_lab
  ```

#### Step 17 Multi-Process Table

| Process | PID | State | CPU % |
| :--- | :---: | :---: | :---: |
| **Process 1** | `3160` | `S+` | `0.0%` |
| **Process 2** | `3203` | `S+` | `0.0%` |
| **Process 3** | `3282` | `S+` | `0.0%` |

*(Note: The `grep process_lab` search process itself appeared as PID `3502` and is excluded from the application process table above.)*

---

## 5. Analysis Questions & Answers

1. **Are the PIDs the same?**  
   **Answer:** No. Each running instance was assigned a completely distinct PID (`3160`, `3203`, and `3282`).

2. **Why are the PIDs different?**  
   **Answer:** Every time a program is launched, the operating system creates a new, independent process in system memory. To manage, schedule, and allocate isolated virtual memory spaces to each instance safely, the OS kernel assigns a unique Process ID to every individual instance.

3. **Are the three processes running the same program?**  
   **Answer:** Yes. All three processes are executing the exact same Python source code file (`process_lab.py`).

4. **Can one program create multiple processes?**  
   **Answer:** Yes. A program file stored on disk is a passive entity (a set of executable instructions). Multiple active processes can be created from a single program file whenever it is launched multiple times concurrently, or when a running process calls system APIs (such as `fork()`) to spawn child processes.

5. **Who manages these processes?**  
   **Answer:** The **Operating System kernel** (specifically the Process Manager and CPU Scheduler) manages all processes. It controls state transitions, allocates memory pages, handles CPU time-slicing via context switching, and handles process termination.

---

## 6. Visual Evidence & Screenshots

*(Instructions: Paste your image links, upload your screenshot files into a `/screenshots` folder in your repository, or attach your images under each corresponding section below.)*

### Screenshot 1: GitHub Repository
![1. GitHub Repository](./screenshots/01_github_repository.png)  
*Caption: GitHub repository `OS-Laboratory-1` created on GitHub.*

---


### Screenshot 2: GitHub Codespaces Environment
<img width="1919" height="986" alt="codespace" src="https://github.com/user-attachments/assets/24912230-d4d7-498d-9dff-7a6b6702f7bc" />
Caption: Active GitHub Codespaces development environment.

---

### Screenshot 3: Python Source Code
![3. Python Source Code](./screenshots/03_python_source_code.png)  
*Caption: Source code of `process_lab.py` open in the VS Code editor.*

---

### Screenshot 4: Program Execution Showing PID
![4. Program Execution Showing PID](./screenshots/04_program_execution_pid.png)  
*Caption: Terminal output displaying process creation and assigned PID.*

---

### Screenshot 5: `ps aux` Output
![5. ps aux Output](./screenshots/05_ps_aux_output.png)  
*Caption: `ps aux` command output showing process metrics.*

---

### Screenshot 6: `ps -o pid,stat,cmd` Output
![6. ps -o pid,stat,cmd Output](./screenshots/06_ps_stat_cmd_output.png)  
*Caption: Process state codes (`STAT`) output.*

---

### Screenshot 7: `top` Output
![7. top Output](./screenshots/07_top_output.png)  
*Caption: Real-time resource monitoring using the `top` command.*

---

### Screenshot 8: Multiple Instances of `process_lab.py`
![8. Multiple Instances of process_lab.py](./screenshots/08_multiple_instances.png)  
*Caption: `ps aux` output showing multiple concurrent instances of `process_lab.py` with unique PIDs.*
