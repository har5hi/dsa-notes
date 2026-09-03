# LeetCode 435 - Non-overlapping Intervals

## 📌 Problem Statement

You are given an array of intervals where:

```text
intervals[i] = [starti, endi]
```

Return the **minimum number of intervals you need to remove** so that the remaining intervals are **non-overlapping**.

Two intervals overlap if:

```text
current.start < previous.end
```

---

## Example 1

```text
Input

intervals = [[1,2],[2,3],[3,4],[1,3]]

Output

1
```

### Explanation

Remove

```text
[1,3]
```

Remaining

```text
[1,2]
[2,3]
[3,4]
```

No overlaps remain.

---

## Example 2

```text
Input

intervals = [[1,2],[1,2],[1,2]]

Output

2
```

---

## Example 3

```text
Input

intervals = [[1,2],[2,3]]

Output

0
```

---

# 💡 Intuition

Instead of thinking:

> Which intervals should I remove?

Think:

> **Which maximum number of intervals can I keep?**

If we maximize the number of intervals we keep,

then

```text
Removed = Total - Kept
```

Now the problem becomes exactly like **Activity Selection / N Meetings in One Room**.

To keep the maximum number of non-overlapping intervals,

always keep the interval that **ends earliest**.

---

# Optimal Approach (Greedy)

### Step 1

Sort intervals according to

```text
Ending Time
```

---

### Step 2

Always keep the first interval.

---

### Step 3

Traverse remaining intervals.

If

```text
current.start >= previousEnd
```

keep it.

Otherwise

```text
remove it
```

---

### Step 4

Return number of removed intervals.

---

# C++ Code

```cpp
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {

        sort(intervals.begin(), intervals.end(),
        [](vector<int>& a, vector<int>& b) {

            return a[1] < b[1];
        });

        int removed = 0;
        int prevEnd = intervals[0][1];

        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] >= prevEnd) {
                prevEnd = intervals[i][1];
            }
            else {
                removed++;
            }
        }
        return removed;
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

# 🔍 Line-by-Line Explanation

```cpp
sort(intervals.begin(), intervals.end(),
```

Sort intervals.

---

```cpp
return a[1] < b[1];
```

Sort according to ending time.

---

```cpp
int removed = 0;
```

Stores the number of intervals removed.

---

```cpp
int prevEnd = intervals[0][1];
```

Keep the first interval.

Store its ending time.

---

```cpp
for(int i=1;i<intervals.size();i++)
```

Check remaining intervals.

---

```cpp
if(intervals[i][0] >= prevEnd)
```

Current interval doesn't overlap.

Keep it.

---

```cpp
prevEnd = intervals[i][1];
```

Update ending time.

---

```cpp
else
```

Overlap detected.

---

```cpp
removed++;
```

Remove the current interval.

---

```cpp
return removed;
```

Minimum removals required.

---

# 🎯 Why Greedy Works?

Suppose two overlapping intervals:

```text
A

1 --------- 8
```

```text
B

2 --3
```

Keeping

```text
A
```

blocks many future intervals.

Keeping

```text
B
```

leaves more room for later intervals.

Therefore,

among overlapping intervals,

keeping the one that **ends earliest** is always the optimal greedy choice.

Since we maximize the number of intervals kept,

we automatically minimize the number removed.

---