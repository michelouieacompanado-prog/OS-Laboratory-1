# OS Laboratory 1: Process Creation, Process States, PCB, and Context Switching

**Course:** BCA151 Operating Systems Lab  
**Repository:** `OS-Laboratory-1`  
**Description:** Operating Systems Laboratory 1 – Process Management  

---

## 📌 Overview

A **process** is an instance of a computer program being executed by an operating system. When a program starts, the OS creates a process, assigns it a unique Process ID (PID), allocates necessary system resources, and manages its lifecycle. 

In this laboratory activity, GitHub Codespaces and Linux CLI tools (`ps`, `top`, `grep`) are used to observe running Python processes, identify process states, inspect process attributes mapped to the **Process Control Block (PCB)**, and explore multi-process execution and CPU scheduling.

---

## 🎯 Learning Outcomes

After completing this laboratory activity, you will be able to:
1. Create and manage an Operating Systems laboratory repository using GitHub.
2. Use GitHub Codespaces to develop and execute Python scripts.
3. Demonstrate process creation by running a Python process.
4. Identify and interpret the Process ID (PID) assigned to a process.
5. Monitor running processes using basic Linux commands (`ps`, `top`, `grep`).
6. Observe and describe process state transitions during execution.
7. Relate observed process information to the **Process Control Block (PCB)**.
8. Explain the role of CPU scheduling in managing multiple concurrent processes.
9. Demonstrate and explain context switching based on experimental observations.
10. Analyze the interplay between processes, process states, PCB, CPU scheduling, and context switching.

---

## 🛠️ Prerequisites & Requirements

* Existing GitHub Account
* Access to GitHub Codespaces
* Web Browser with an internet connection
* Basic familiarity with Python and Linux CLI tools

---

## 💻 Source Code (`process_lab.py`)

Create a file named `process_lab.py` in your workspace root directory and add the following code:

```python
import os
import time

print("========================================")
print("     OPERATING SYSTEMS LABORATORY")
print("     PROCESS MONITORING ACTIVITY")
print("========================================")

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

## 🚀 Step-by-Step Procedure

### 1. Environment & Setup
1. Sign in to GitHub and create a new private repository named **`OS-Laboratory-1`**.
2. Launch a GitHub Codespaces environment on the `main` branch.
3. Save `process_lab.py` and commit your code with the message: `"Create process laboratory program"`.

### 2. Execution & Process ID
1. Open a new terminal in Codespaces (`Terminal` → `New Terminal`).
2. Verify Python installation:
   ```bash
   python3 --version
   ```
3. Execute the script:
   ```bash
   python3 process_lab.py
   ```
4. Record the printed **Process ID (PID)**.

### 3. Process Monitoring (Single Process)
While `process_lab.py` is running, open a **second terminal window** and run the following commands:

* **View basic process status:**
  ```bash
  ps
  ```
* **View detailed process status:**
  ```bash
  ps aux
  ```
* **Filter for the laboratory process:**
  ```bash
  ps aux | grep process_lab
  ```
* **Display PID, State, and Command:**
  ```bash
  ps -o pid,stat,cmd
  ```
* **Interactive Process Monitor:**
  ```bash
  top
  ```
  *(Press `q` to exit `top`)*

### 4. Multi-Process Execution
1. Open **three separate terminal windows**.
2. Run `python3 process_lab.py` in each terminal concurrently.
3. Open a **fourth terminal window** and monitor all active processes:
   ```bash
   ps aux | grep process_lab
   ```

---

## 📊 Observations & Data Logs

### 1. Single Process Information (`ps aux`)

| Attribute | Observed Value |
| :--- | :--- |
| **PID** | |
| **CPU %** | |
| **Memory %**| |
| **User** | |
| **Command** | |

### 2. Linux Process State Legend

| Code | Meaning |
| :---: | :--- |
| **R** | Running / Runnable |
| **S** | Interruptible Sleep (waiting for an event/timer) |
| **D** | Uninterruptible Sleep (usually I/O) |
| **T** | Stopped |
| **Z** | Zombie (terminated but parent hasn't read exit status) |

### 3. Multiple Process Monitoring

| Process Instance | PID | State (`STAT`) | CPU % |
| :--- | :---: | :---: | :---: |
| **Process 1** | | | |
| **Process 2** | | | |
| **Process 3** | | | |

---

## 🧠 Concept Mapping: Process Control Block (PCB)

The operating system maintains information about each process inside a structure called the **Process Control Block (PCB)**.

```
       PROCESS
          │
          ▼
┌───────────────────┐
│        PCB        │
├───────────────────┤
│ Process ID        │
│ Process State     │
│ CPU Context       │
│ Scheduling Info   │
│ Memory Info       │
│ I/O Information   │
└───────────────────┘
```

---

## ❓ Analysis & Discussion Questions

1. **Why does the operating system assign a unique PID to every process?**
   > *Your answer here...*

2. **How does the `grep` command help us locate a specific process when used with `ps aux`?**
   > *Your answer here...*

3. **Which information observed using `ps` and `top` can be associated with data maintained in the internal PCB?**
   > *Your answer here...*

4. **Multi-Process Analysis:**
   * Are the PIDs of the three running instances the same? Why or why not?
   * Are all three processes running the same underlying Python program code?
   * Can a single program produce multiple distinct processes?
   * Which OS component is responsible for managing and scheduling these processes?
   > *Your answer here...*

---

## 🖼️ Required Screenshots Checklist

Ensure the following screenshots are captured and included in your final laboratory report:

- [ ] GitHub repository setup (`OS-Laboratory-1`)
- [ ] Active GitHub Codespaces environment
- [ ] Python source code (`process_lab.py`) open in editor
- [ ] Program execution output displaying assigned PID
- [ ] Output of `ps aux` command
- [ ] Output of `ps -o pid,stat,cmd` command
- [ ] Output of `top` interactive system monitor
- [ ] Multiple terminal instances executing `process_lab.py` concurrently