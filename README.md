
# Multi-Container Runtime

## 🔹 Team Information

* **Varnika Sadineni** — PES1UG24CS515
* **Tarini Arora** — PES1UG24CS495

---

## 🔹 Overview

This project implements a lightweight container runtime in C, inspired by core Linux container principles. It demonstrates how operating system concepts like process isolation, scheduling, and kernel interaction work together in a practical system.

### 🔧 Key Features

* Multi-container execution using Linux namespaces
* Centralized supervisor process
* Per-container logging system
* Kernel-level monitoring using a Loadable Kernel Module (LKM)
* CPU vs I/O scheduling experiments
* Clean lifecycle management (no zombie processes)

---

## 🔹 Build, Load, and Run Instructions

### 1. Build the Project

```bash
cd boilerplate
make
```

---

### 2. Load Kernel Module

```bash
sudo insmod monitor.ko
sudo dmesg | tail -n 20
```

---

### 3. Start Supervisor

```bash
sudo ./engine supervisor ./rootfs-base
```

---

### 4. Prepare Container Root Filesystems

```bash
cp -a ./rootfs-base ./rootfs-alpha
cp -a ./rootfs-base ./rootfs-beta
```

---

### 5. Start Containers

```bash
sudo ./engine start alpha ./rootfs-alpha /cpu_hog
sudo ./engine start beta ./rootfs-beta /io_pulse
```

---

### 6. Inspect Containers

```bash
sudo ./engine ps
```

---

### 7. View Logs

```bash
ls logs
cat logs/alpha.log
```

---

### 8. Stop Containers

```bash
sudo ./engine stop alpha
sudo ./engine stop beta
```

---

## 📸 Demo with Screenshots
1. Multi-container supervision
   <img width="1280" height="768" alt="ss1" src="https://github.com/user-attachments/assets/85588481-e06c-4b90-8e02-1a6e711ea9f1" />

Two containers running concurrently under a single supervisor process

2. Metadata tracking (ps)
   <img width="836" height="252" alt="ss2" src="https://github.com/user-attachments/assets/a82408c6-83b4-4909-ad64-62855fabffa6" />
   Supervisor maintains metadata including container ID, PID, and state
   
3. Bounded-buffer logging
  <img width="1536" height="981" alt="ss3" src="https://github.com/user-attachments/assets/182afead-8e4a-4cc4-8ff6-eeeac5c2e4e8" />
   Container output captured through bounded-buffer logging pipeline

4. CLI + IPC
  <img width="1280" height="761" alt="ss4" src="https://github.com/user-attachments/assets/e7f75562-bb9a-411d-a76a-a26b056b36cc" />
   CLI communicates with supervisor via IPC mechanism

5. Soft-limit warning
   <img width="1536" height="981" alt="ss5" src="https://github.com/user-attachments/assets/6b166238-470e-453d-a3d3-c3b6fdf28653" />

   Soft memory limit exceeded triggers warning without terminating container

6. Hard-limit enforcement
  <img width="1128" height="574" alt="ss6" src="https://github.com/user-attachments/assets/9272b74d-1939-42a1-ae10-e6870df0c841" />
   Hard memory limit enforcement terminates container process

7. Scheduling experiment
   <img width="1536" height="1024" alt="ss7 2" src="https://github.com/user-attachments/assets/4a863214-1d01-4f5a-8350-1e25b2a527bb"/><img width="1536" height="1024" alt="ss7 1" src="https://github.com/user-attachments/assets/7fa81226-0140-450b-afee-f5bdcf1e6d05" />

   Linux scheduler prioritizes lower nice value (higher priority) container

8. Clean teardown
   <img width="1536" height="1024" alt="ss8" src="https://github.com/user-attachments/assets/9f07c1d8-9eb2-4430-be07-27a096820d1f" />
   All containers terminated cleanly with no zombie processes remaining

## 🔹 Engineering Analysis

### 🔹 Process Isolation

Containers are created using Linux namespaces:

* Separate process trees
* Isolated filesystem environments
* Independent execution contexts

---

### 🔹 Supervisor Design

The supervisor acts as:

* A central controller for container lifecycle
* A process manager for spawning and stopping containers
* A coordinator for logging and monitoring

---

### 🔹 Logging System

* Each container writes to its own log file
* Output is captured via pipes
* Logs persist even after container termination

---

### 🔹 Kernel Monitoring

* Implemented as a Loadable Kernel Module
* Interacts with user-space runtime
* Demonstrates kernel-user communication

---

### 🔹 Scheduling Behavior

* CPU-bound processes utilize maximum CPU
* I/O-bound processes frequently yield CPU
* Demonstrates real-world Linux scheduling behavior

---

## 🔹 Design Decisions & Tradeoffs

### Container Isolation

* **Approach:** Namespace-based
* **Tradeoff:** Lightweight but less secure than full container runtimes

### Supervisor

* **Approach:** Single process controller
* **Tradeoff:** Simple design but single point of failure

### Logging

* **Approach:** File-based logging
* **Tradeoff:** Easy implementation, limited scalability

### Kernel Monitoring

* **Approach:** LKM-based tracking
* **Tradeoff:** Powerful but increases complexity

---

## 🔹 Observations

### CPU vs IO

* `cpu_hog` consumes significantly higher CPU
* `io_pulse` uses minimal CPU

### System Behavior

* Containers execute and terminate cleanly
* Logs are consistently generated
* No zombie processes observed

---

##  Notes

* All outputs captured from a Linux VM environment
* Kernel module successfully loads and initializes
* Some container commands may exit quickly, but system behavior remains consistent

---

##  Conclusion

This project demonstrates a simplified container runtime that integrates:

* Process isolation
* Logging pipeline
* Kernel interaction
* Scheduling behavior

It provides a strong practical understanding of how operating systems manage processes, resources, and execution environments.

   

   

   
