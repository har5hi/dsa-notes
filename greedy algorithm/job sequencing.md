# Job Sequencing Problem (Greedy Algorithm)

---

# 📌 Problem Statement

You are given `N` jobs.

Each job has:

- **Job ID**
- **Deadline**
- **Profit**

Each job takes **exactly 1 unit of time**.

Only **one job** can be performed at a time.

A job must be completed **on or before its deadline** to earn its profit.

Your task is to maximize:

1. **Number of jobs completed**
2. **Total profit earned**

Return:

```text
[number_of_jobs, maximum_profit]
```

---

## Example 1

```text
Input

Jobs

ID  Deadline Profit

1      4       20
2      1       10
3      1       40
4      1       30
```

Output

```text
[2,60]
```

### Explanation

Schedule

```text
Time 1 → Job 3 (Profit 40)

Time 4 → Job 1 (Profit 20)
```

Total

```text
Jobs = 2

Profit = 60
```

---

## Example 2

```text
Input

ID  Deadline Profit

1      2      100
2      1      19
3      2      27
4      1      25
5      3      15
```

Output

```text
[3,142]
```

---

# 💡 Intuition

Suppose two jobs have the same deadline.

```
Job A

Profit = 100
```

```
Job B

Profit = 20
```

If only one can be completed,

which one should we choose?

Obviously,

```
Job A
```

because it gives more profit.

Now,

where should we place it?

To avoid blocking earlier time slots,

always place the job in the **latest available slot before its deadline**.

This leaves earlier slots free for other jobs.

This is the greedy strategy.

---

# Optimal Approach (Greedy)

### Step 1

Sort jobs in **descending order of profit**.

---

### Step 2

Find the maximum deadline.

Create time slots:

```text
1 ... maxDeadline
```

Initially,

all slots are empty.

---

### Step 3

For every job (highest profit first):

Try to schedule it in the **latest free slot** before its deadline.

If found,

assign the job.

Otherwise,

skip it.

---

# Algorithm

1. Sort jobs by profit (descending).
2. Find maximum deadline.
3. Create a slot array initialized with `-1`.
4. Traverse every job.
5. From its deadline to slot `1`:
   - If a slot is free:
     - Assign the job.
     - Increase job count.
     - Add profit.
     - Break.
6. Return

```text
[count, profit]
```

---

# C++ Code (GFG)

```cpp
class Solution {
public:

    vector<int> JobScheduling(Job arr[], int n) {

        sort(arr, arr + n, [](Job &a, Job &b) {
            return a.profit > b.profit;
        });

        int maxDeadline = 0;

        for (int i = 0; i < n; i++) {
            maxDeadline = max(maxDeadline, arr[i].dead);
        }

        vector<int> slot(maxDeadline + 1, -1);

        int count = 0;
        int profit = 0;

        for (int i = 0; i < n; i++) {
            for (int j = arr[i].dead; j > 0; j--) {

                if (slot[j] == -1) {

                    slot[j] = arr[i].id;
                    count++;
                    profit += arr[i].profit;

                    break;
                }
            }
        }
        return {count, profit};
    }
};
```

---

# ⏱ Time Complexity

Sorting

```text
O(n log n)
```

Scheduling

```text
O(n × D)
```

Where

```text
D = Maximum Deadline
```

Worst case

```text
O(n²)
```

---

# 📦 Space Complexity

Slot array

```text
O(D)
```

---

# 🎯 Why Greedy Works?

The greedy strategy has **two important choices**:

### 1. Pick the highest-profit job first.

If two jobs compete for the same slot, keeping the one with higher profit always gives a better result.

---

### 2. Schedule it as late as possible.

Example

```
Deadline = 4
```

Instead of placing the job at time `1`,

place it at time `4`.

This keeps earlier slots free for jobs with smaller deadlines.

Thus,

- High-profit jobs are never unnecessarily rejected.
- Earlier time slots remain available for other jobs.

Together, these choices maximize the total profit.

---

# 🚀 Optimized Approach (Using Disjoint Set Union - DSU)

The above solution checks slots backward one by one.

This can become **O(n²)** in the worst case.

A faster approach uses **Disjoint Set Union (Union-Find)**.

### Idea

- Each slot belongs to a set.
- When a slot is occupied, union it with the previous slot.
- Finding the latest available slot becomes nearly constant time.

### Complexity

| Approach | Time Complexity |
|----------|-----------------|
|Greedy + Slot Array|O(n × D)|
|Greedy + DSU|O(n log n) (sorting) + O(n α(n))|

In interviews, the **slot-array solution** is expected first. Mention the **DSU optimization** if asked about improving the worst-case complexity.

---