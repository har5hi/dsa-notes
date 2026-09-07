# Shortest Job First (SJF) CPU Scheduling (Greedy Algorithm)

> **Problem Source:** GeeksforGeeks (GFG)

---

# 📌 Problem Statement

Given the **burst time** of `N` processes, schedule them using the **Shortest Job First (SJF)** algorithm.

The goal is to minimize:

- Average Waiting Time
- Average Turnaround Time

> **Assumption (Non-Preemptive SJF):**
>
> - All processes arrive at time **0**.
> - Once a process starts executing, it cannot be interrupted.

---

## Example

```text
Burst Time

P1 = 6
P2 = 8
P3 = 7
P4 = 3
```

Output

```text
Execution Order

P4 → P1 → P3 → P2
```

Average Waiting Time

```text
7
```

---

# Important Terminology

## 1. Burst Time (BT)

Time required by a process to execute.

---

## 2. Arrival Time (AT)

Time at which a process enters the ready queue.

In this problem,

```text
Arrival Time = 0
```

for every process.

---

## 3. Completion Time (CT)

Time at which the process finishes execution.

---

## 4. Turnaround Time (TAT)

```text
TAT = Completion Time − Arrival Time
```

---

## 5. Waiting Time (WT)

Time spent waiting before execution.

```text
WT = Turnaround Time − Burst Time
```

Since

```text
Arrival = 0
```

we can also compute

```text
WT = Start Time
```

---

# 💡 Intuition

Suppose two processes are waiting.

```
P1

Burst = 2
```

```
P2

Burst = 20
```

If we execute

```
P2
```

first,

the small process waits for a long time.

Instead,

if we execute

```
P1
```

first,

both processes finish earlier on average.

Hence,

always execute the process with the **smallest burst time** first.

This is the greedy choice.

---

# Optimal Approach (Greedy)

### Step 1

Sort processes according to

```text
Burst Time
```

---

### Step 2

Execute them in that order.

---

### Step 3

Waiting time of each process equals

```text
Sum of burst times of all previous processes.
```

---

### Step 4

Compute

Average Waiting Time.

---

# 📝 Algorithm

1. Sort burst times.
2. Initialize

```text
currentTime = 0

totalWaiting = 0
```

3. Traverse processes.

For every process

- Waiting Time = currentTime
- Add to answer
- Update

```text
currentTime += burstTime
```

4. Return

```text
Average Waiting Time
```

---

# ✅ C++ Code (GFG)

```cpp
class Solution {
public:
    long long solve(vector<int>& bt) {

        sort(bt.begin(), bt.end());

        long long waitingTime = 0;
        long long currentTime = 0;

        for (int i = 0; i < bt.size(); i++) {
            waitingTime += currentTime;
            currentTime += bt[i];
        }
        return waitingTime / bt.size();
    }
};
```

---

# ⏱ Time Complexity

Sorting

```text
O(n log n)
```

Traversal

```text
O(n)
```

Overall

```text
O(n log n)
```

---

# 📦 Space Complexity

Only variables are used.

```text
O(1)
```

(ignoring sorting space)

---

# ▶️ Dry Run

### Input

```text
Burst Times

6 8 7 3
```

---

### Step 1

Sort

```text
3 6 7 8
```

---

### Execution Order

```
P4

↓

P1

↓

P3

↓

P2
```

---

### Waiting Time Table

| Process | Burst | Waiting Time |
|---------|------:|-------------:|
|P4|3|0|
|P1|6|3|
|P3|7|9|
|P2|8|16|

---

Total Waiting Time

```text
0 + 3 + 9 + 16

= 28
```

Average

```text
28 / 4

= 7
```

Answer

```text
7
```

---

# 🔍 Line-by-Line Explanation

```cpp
sort(bt.begin(), bt.end());
```

Execute shortest jobs first.

---

```cpp
long long waitingTime = 0;
```

Stores total waiting time.

---

```cpp
long long currentTime = 0;
```

Represents CPU time elapsed.

---

```cpp
waitingTime += currentTime;
```

Current process waits until all previous jobs finish.

---

```cpp
currentTime += bt[i];
```

Execute current process.

---

```cpp
return waitingTime / bt.size();
```

Return average waiting time.

---

# 🎯 Why Greedy Works?

Suppose two jobs:

```
A

Burst = 2
```

```
B

Burst = 10
```

### Option 1

Execute

```
A → B
```

Waiting Times

```
A = 0

B = 2
```

Average

```text
(0 + 2) / 2 = 1
```

---

### Option 2

Execute

```
B → A
```

Waiting Times

```
B = 0

A = 10
```

Average

```text
(0 + 10) / 2 = 5
```

Clearly,

executing the shorter job first minimizes the average waiting time.

This is the greedy strategy behind SJF.

---

# 💼 Interview Tips

### Hint 1

Whenever the problem asks to minimize

- Average Waiting Time
- Average Turnaround Time

Think of **Shortest Job First**.

---

### Hint 2

For the basic GFG version,

all processes arrive at time **0**.

Simply sort by burst time.

---

### Common Mistakes

❌ Forgetting to sort.

❌ Calculating waiting time incorrectly.

❌ Confusing Waiting Time with Turnaround Time.

❌ Assuming arrival times are different when the problem states all are zero.

---

# 🧠 Pattern Recognition

This is a classic **Greedy Scheduling** problem.

### Pattern

- Sort by the smallest processing time.
- Execute in that order.
- Accumulate waiting time.

Similar Problems:

- Job Sequencing
- Activity Selection
- Fractional Knapsack
- CPU Scheduling Algorithms
- Scheduling Optimization

---

# ⚖️ SJF vs Job Sequencing

| Feature | SJF | Job Sequencing |
|---------|-----|----------------|
|Sorting|Burst Time ↑|Profit ↓|
|Goal|Minimum Waiting Time|Maximum Profit|
|Execution|Every process executes|Some jobs may be skipped|
|Duration|Variable|Always 1 unit|

---

# 🚀 SJF Variants

### 1. Non-Preemptive SJF

- Once a process starts, it finishes completely.
- Easier to implement.
- Solved by sorting burst times (when all arrival times are 0).

---

### 2. Preemptive SJF (Shortest Remaining Time First - SRTF)

- A newly arrived process with a smaller remaining burst time can interrupt the current process.
- More complex.
- Uses a priority queue/min-heap.

---

# 🎯 Visual Understanding

```
Burst Times

6   8   7   3

↓

Sort

3   6   7   8

↓

CPU Timeline

|---P4---|------P1------|-------P3-------|--------P2--------|
0        3             9              16                 24
```

Waiting Times

```
P4 = 0
P1 = 3
P3 = 9
P2 = 16
```

Average Waiting Time

```
7
```

---