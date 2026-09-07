# LeetCode 57 - Insert Interval

## 📌 Problem Statement

You are given:

- A list of **non-overlapping intervals** sorted in ascending order by their start time.
- A new interval `newInterval`.

Insert the new interval into the list such that:

- The intervals remain sorted.
- Overlapping intervals are merged.

Return the updated list of intervals.

---

## Example 1

```text
Input

intervals = [[1,3],[6,9]]

newInterval = [2,5]

Output

[[1,5],[6,9]]
```

## Example 2

```text
Input

intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]]

newInterval = [4,8]

Output

[[1,2],[3,10],[12,16]]
```

---

# 💡 Intuition

Since the intervals are already **sorted** and **non-overlapping**, only the new interval can create overlaps.
Every interval belongs to one of three categories:

### Case 1

Completely before the new interval.

```
[1,2]

          [5,7]
```

Keep it as it is.

---

### Case 2

Completely after the new interval.

```
      [2,4]

[6,8]
```

The new interval is finalized.
Insert it and copy the remaining intervals.

---

### Case 3

Overlapping.

```
[2,5]

   [4,8]
```

Merge by taking:

```
Start = min(start)
End = max(end)
```

---

# ❌ Brute Force Approach

Insert the new interval into the array.

Sort all intervals.

Merge all intervals.

### Time Complexity

```text
O(n log n)
```

Sorting is unnecessary because the intervals are already sorted.

---

# Optimal Approach (Greedy)

Process intervals in three phases.

---

### Phase 1

Copy all intervals completely before the new interval.

Condition

```text
interval.end < new.start
```

---

### Phase 2

Merge all overlapping intervals.

Condition

```text
interval.start <= new.end
```

Update

```text
new.start = min(...)

new.end = max(...)
```

---

### Phase 3

Insert the merged interval.
Then copy all remaining intervals.

---

# C++ Code

```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> ans;

        int i = 0;
        int n = intervals.size();

        while (i < n && intervals[i][1] < newInterval[0]) {
            ans.push_back(intervals[i]);
            i++;
        }

        while (i < n && intervals[i][0] <= newInterval[1]) {
            newInterval[0] = min(newInterval[0], intervals[i][0]);
            newInterval[1] = max(newInterval[1], intervals[i][1]);
            i++;
        }

        ans.push_back(newInterval);

        while (i < n) {
            ans.push_back(intervals[i]);
            i++;
        }
        return ans;
    }
};
```

---

# ⏱ Time Complexity

Single traversal.

```text
O(n)
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
while(intervals[i][1] < newInterval[0])
```

Current interval lies completely before the new interval.

Copy it.

---

```cpp
ans.push_back(intervals[i]);
```

Add interval unchanged.

---

```cpp
while(intervals[i][0] <= newInterval[1])
```

Current interval overlaps with the new interval.

---

```cpp
newInterval[0] =
min(newInterval[0], intervals[i][0]);
```

Update merged start.

---

```cpp
newInterval[1] =
max(newInterval[1], intervals[i][1]);
```

Update merged end.

---

```cpp
ans.push_back(newInterval);
```

Insert the merged interval.

---

```cpp
while(i < n)
```

Copy remaining intervals.

---

# 🎯 Why Greedy Works?

Since the original intervals are:

- Already sorted
- Already non-overlapping

we never need to revisit earlier intervals.
Every interval is processed exactly once.
There are only three possibilities:

```
Before
↓
Merge
↓
After
```

As soon as we finish merging, the merged interval is finalized forever.
No future interval can affect previous intervals.
This makes the greedy approach optimal.

---