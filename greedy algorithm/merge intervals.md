# LeetCode 56 - Merge Intervals

## 📌 Problem Statement

You are given an array of intervals where:

```text
intervals[i] = [starti, endi]
```

Merge all overlapping intervals and return an array of the non-overlapping intervals that cover all the intervals in the input.

---

## Example 1

```text
Input

intervals = [[1,3],[2,6],[8,10],[15,18]]

Output

[[1,6],[8,10],[15,18]]
```

---

## Example 2

```text
Input

intervals = [[1,4],[4,5]]

Output

[[1,5]]
```

---

# 💡 Intuition

If the intervals are **sorted by their starting time**, then:

- Any interval can only overlap with the **last merged interval**.
- We never need to compare it with all previous intervals.

So the process becomes:

- Sort the intervals.
- Keep one merged interval.
- If the next interval overlaps, extend the current interval.
- Otherwise, start a new interval.

This greedy strategy guarantees the correct answer.

---

# Brute Force Approach

Compare every interval with every other interval.
Merge whenever overlaps exist.
Repeat until no overlaps remain.

### Time Complexity

```text
O(n²)
```

Very inefficient.

---

# Optimal Approach (Greedy + Sorting)

### Step 1

Sort intervals according to

```text
Starting Time
```

---

### Step 2

Insert the first interval into the answer.

---

### Step 3

Traverse remaining intervals.

If

```text
current.start <= lastMerged.end
```

merge them.

Otherwise, add a new interval.

---

# C++ Code

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {

        sort(intervals.begin(), intervals.end());
        vector<vector<int>> ans;
        ans.push_back(intervals[0]);

        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] <= ans.back()[1]) {
                ans.back()[1] = max(ans.back()[1], intervals[i][1]);
            }
            else {
                ans.push_back(intervals[i]);
            }
        }
        return ans;
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

Answer vector

```text
O(n)
```

---

# 🔍 Line-by-Line Explanation

```cpp
sort(intervals.begin(), intervals.end());
```

Sort intervals according to their starting time.

---

```cpp
vector<vector<int>> ans;
```

Stores the merged intervals.

---

```cpp
ans.push_back(intervals[0]);
```

Insert the first interval.

---

```cpp
if(intervals[i][0] <= ans.back()[1])
```

Current interval overlaps with the last merged interval.

---

```cpp
ans.back()[1] =
max(ans.back()[1], intervals[i][1]);
```

Extend the ending time.

---

```cpp
else
```

No overlap.

---

```cpp
ans.push_back(intervals[i]);
```

Start a new merged interval.

---

```cpp
return ans;
```

Return all merged intervals.

---

# 🎯 Why Greedy Works?

After sorting by start time,

suppose the last merged interval is

```
[1,6]
```

The next interval is

```
[2,5]
```

or

```
[5,9]
```

or

```
[8,10]
```

Since all future intervals start later,

only the **last merged interval** can overlap with the current one.

If they overlap,

extend the end.

Otherwise,

the previous merged interval is finalized forever.

Thus,

we never need to compare against earlier intervals.

This makes the greedy solution optimal.

---