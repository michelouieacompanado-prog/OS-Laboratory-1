# OS-Laboratory-1: Process Management

**Course:** BCA151 Operating Systems Lab  
**Activity:** Lab Activity No. 1 – Process Creation, Process States, Process Control Block, and Context Switching  
**Environment:** GitHub Codespaces (Ubuntu Linux Container)

---

## Overview

This repository contains the source code, monitoring logs, and formal laboratory report for **Lab Activity No. 1** in Operating Systems. The objective of this activity is to observe process creation, monitor process execution metrics, analyze process state transitions, map terminal observations to kernel **Process Control Blocks (PCBs)**, and execute concurrent process instances in a Linux environment.

---

## Repository Structure

```text
OS-Laboratory-1/
│
├── README.md           # Project overview, instructions, and OS concepts summary
├── process_lab.py      # Python lab script simulating an active process loop
└── labreport.md        # Complete, formatted laboratory report with empirical data
```

---

## Python Script (`process_lab.py`)

The script fetches its own system Process ID (PID) via `os.getpid()` and executes a timed 15-step loop (`time.sleep(2)`) to keep the process alive in memory for process inspection using `ps` and `top`.

```python
import os
import time

print("==========================================")
print("     OPERATING SYSTEMS LABORATORY")
print("     PROCESS MONITORING ACTIVITY")
print("==========================================")

pid = os.getpid()

print("Process ID (PID):", pid)
print("Process has been created.")
print("Process is executing...")
print()

for i in range(1, 16):
    print("Execution step:", i)
    time.sleep(2)

print()
print("Process execution completed.")
print("Process is terminating...")
```

---

## Quick Start / How to Run

1. **Clone or Open in GitHub Codespaces:**
   Open this repository directly inside GitHub Codespaces.

2. **Run a Single Instance:**
   ```bash
   python process_lab.py
   ```

3. **Monitor Running Processes:**
   In a separate terminal tab, enter any of the following commands:
   * **List active process:** `ps aux | grep process_lab`
   * **Inspect process states:** `ps -o pid,stat,cmd`
   * **Real-time resource monitor:** `top` (Press `q` to exit)

4. **Run Multiple Concurrent Instances:**
   Open three separate terminal tabs and run `python process_lab.py` in each. Use a fourth tab to query all three running instances simultaneously:
   ```bash
   ps aux | grep process_lab
   ```

---

## Key OS Concepts Covered

* **Process Creation & PIDs:** How the kernel instantiates processes and assigns unique integer IDs.
* **Linux Process States:** Decoding state symbols (`R` for Running, `S` for Interruptible Sleep, `+` for Foreground Group).
* **Process Control Block (PCB):** Linking CLI output (`PID`, `%CPU`, `%MEM`, `STAT`, `TTY`) to OS kernel process metadata structures.
* **Concurrency & Context Switching:** Observing how the Linux scheduler manages multiple independent processes executing the same binary file simultaneously.
