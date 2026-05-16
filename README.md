# CPU Scheduling Algorithm Simulator

Implements and compares five fundamental CPU scheduling algorithms in C, measuring average turnaround time across different process workloads.

---

## Algorithms

**[FCFS — First-Come, First-Served](main.c)**
Non-preemptive. Processes run in arrival order until completion.
When it's useful: batch systems where fairness by arrival order matters.
_Learned: prone to the "convoy effect" — one long job blocks all short jobs behind it, inflating average turnaround time._

---

**[LCFS-NP — Last-Come, First-Served (Non-Preemptive)](main.c)**
Non-preemptive stack (LIFO). Most recently arrived process runs next; once started, runs to completion.
When it's useful: illustrates how queue discipline (FIFO vs LIFO) changes behavior with no added complexity.
_Learned: early-arriving processes can starve if new ones keep arriving — LIFO ordering makes starvation far worse than FIFO._

---

**[LCFS-P — Last-Come, First-Served (Preemptive)](main.c)**
Preemptive stack. Any newly arrived process immediately preempts the running one by jumping to the top of the stack.
When it's useful: demonstrates how preemption interacts with LIFO ordering — combining both maximizes context switches.
_Learned: simulated in 1-unit time steps; the gap between theory (instant preemption) and practice (context-switch overhead) becomes obvious at this granularity._

---

**[RR — Round Robin (quantum = 2)](main.c)**
Preemptive circular queue. Each process gets at most 2 time units per turn; incomplete processes re-enter the rear of the queue.
When it's useful: interactive and time-sharing systems where response time matters equally for all processes.
_Learned: quantum size is the critical tuning knob — too small and context-switch overhead dominates, too large and it degenerates into FCFS._

---

**[SJF — Shortest Job First (Non-Preemptive)](main.c)**
Non-preemptive. At each scheduling decision, picks the ready process with the shortest remaining burst time.
When it's useful: batch workloads where burst times are known in advance and minimizing average turnaround is the goal.
_Learned: SJF is provably optimal for average turnaround time, but requires knowing future burst lengths — in practice, systems estimate them via exponential averaging._

---

## How to Run

```bash
# Compile
gcc -o scheduler main.c

# Run with a test input
./scheduler input1.txt
```

Expected output format:
```
FCFS: mean turnaround = X.XX
LCFS (NP): mean turnaround = X.XX
LCFS (P): mean turnaround = X.XX
RR: mean turnaround = X.XX
SJF: mean turnaround = X.XX
```

---

## Input Format

```
<number_of_processes>
<arrival_time>,<computation_time>
...
```

Example (`input1.txt` — 4 processes):
```
4
3,5
5,8
1,10
6,9
```

---

## Test Cases

| File | Processes | What it tests |
|------|-----------|---------------|
| input1.txt | 4 | Basic mixed workload |
| input2.txt | 8 | Larger set, staggered arrivals |
| input3.txt | 2 | Sparse workload with long gap between arrivals |
| input4.txt | 4 | Multiple processes with same arrival time |
| input5.txt | 4 | Edge case: processes with zero computation time |

---

## Folder Structure

```
cpu-scheduling-algo-simulator/
├── main.c          # All five scheduling algorithm implementations
├── input1-5.txt    # Test cases with varying process counts and patterns
└── README.md
```
