# LeetCode 56 - Merge Intervals
---

# Problem Statement

Given an array of intervals where:

```cpp
intervals[i] = [starti, endi]
```

Merge all overlapping intervals and return an array of the non-overlapping intervals that cover all the intervals in the input.

---

## Example

### Input

```cpp
intervals = [[1,3],[2,6],[8,10],[15,18]]
```

### Output

```cpp
[[1,6],[8,10],[15,18]]
```

### Explanation

- `[1,3]` and `[2,6]` overlap.
- Merge them into `[1,6]`.

---

# Brute Force Approach

## Idea

For every interval:

- Compare it with the following intervals.
- Keep extending the current interval if overlaps exist.
- Mark merged intervals as visited.
- Add final merged interval to answer.

---

## Steps

1. Sort intervals according to start time.
2. Traverse each interval.
3. Merge all overlapping intervals ahead.
4. Store merged interval.
5. Continue until all intervals are processed.

---

## Code

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {

        int n = intervals.size();
        sort(intervals.begin(), intervals.end());

        vector<vector<int>> ans;

        for(int i = 0; i < n; i++) {

            int start = intervals[i][0];
            int end = intervals[i][1];

            if(!ans.empty() && end <= ans.back()[1])
                continue;

            for(int j = i + 1; j < n; j++) {

                if(intervals[j][0] <= end)
                    end = max(end, intervals[j][1]);
                else
                    break;
            }

            ans.push_back({start, end});
        }

        return ans;
    }
};
```

---

## Dry Run

### Input

```cpp
[[1,3],[2,6],[8,10],[15,18]]
```

After sorting:

```cpp
[[1,3],[2,6],[8,10],[15,18]]
```

### i = 0

```cpp
start = 1
end = 3
```

Check next interval:

```cpp
[2,6]
```

Since:

```cpp
2 <= 3
```

Merge:

```cpp
end = max(3,6) = 6
```

Next:

```cpp
[8,10]
```

Since:

```cpp
8 > 6
```

Stop.

Add:

```cpp
[1,6]
```

---

### i = 1

Already covered by previous merged interval.

Skip.

---

### i = 2

Add:

```cpp
[8,10]
```

---

### i = 3

Add:

```cpp
[15,18]
```

---

### Final Answer

```cpp
[[1,6],[8,10],[15,18]]
```

---

## Complexity Analysis

### Time Complexity

Sorting:

```cpp
O(N log N)
```

Nested traversal:

```cpp
O(N²)
```

Overall:

```cpp
O(N²)
```

### Space Complexity

```cpp
O(N)
```

for answer array.

---

# Optimal Approach

## Observation

After sorting:

- Overlapping intervals always appear next to each other.
- We only need to compare the current interval with the last merged interval.

Instead of checking all future intervals repeatedly:

- Maintain the last merged interval.
- Extend it whenever overlap occurs.

---

## Key Logic

Suppose last merged interval is:

```cpp
[lastStart, lastEnd]
```

Current interval:

```cpp
[currStart, currEnd]
```

### Overlap Condition

```cpp
currStart <= lastEnd
```

Then merge:

```cpp
lastEnd = max(lastEnd, currEnd)
```

---

### No Overlap

```cpp
currStart > lastEnd
```

Push current interval into answer.

---

## Algorithm

1. Sort intervals.
2. Push first interval into answer.
3. Traverse remaining intervals.
4. If overlap:
   - Update ending point.
5. Else:
   - Push new interval.
6. Return answer.

---

## Optimal Code

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {

        sort(intervals.begin(), intervals.end());

        vector<vector<int>> ans;

        ans.push_back(intervals[0]);

        for(int i = 1; i < intervals.size(); i++) {

            if(intervals[i][0] <= ans.back()[1]) {

                ans.back()[1] =
                    max(ans.back()[1], intervals[i][1]);
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

# Detailed Dry Run

### Input

```cpp
[[1,3],[2,6],[8,10],[15,18]]
```

After sorting:

```cpp
[[1,3],[2,6],[8,10],[15,18]]
```

---

### Initialize

```cpp
ans = [[1,3]]
```

---

### i = 1

Current:

```cpp
[2,6]
```

Check overlap:

```cpp
2 <= 3
```

Yes.

Update:

```cpp
ans.back()[1] = max(3,6)
              = 6
```

Now:

```cpp
ans = [[1,6]]
```

---

### i = 2

Current:

```cpp
[8,10]
```

Check:

```cpp
8 <= 6
```

False.

Push interval:

```cpp
ans = [[1,6],[8,10]]
```

---

### i = 3

Current:

```cpp
[15,18]
```

Check:

```cpp
15 <= 10
```

False.

Push:

```cpp
ans = [[1,6],[8,10],[15,18]]
```

---

### Final Answer

```cpp
[[1,6],[8,10],[15,18]]
```

---

# Why Sorting is Necessary?

Consider:

```cpp
[[8,10],[1,3],[2,6]]
```

Without sorting:

```cpp
[8,10]
```

comes before

```cpp
[1,3]
```

and overlap detection becomes difficult.

Sorting guarantees:

```cpp
[[1,3],[2,6],[8,10]]
```

so all overlapping intervals appear together.

---

# Interview Explanation (30 Seconds)

1. Sort intervals by starting time.
2. Store first interval in answer.
3. For every next interval:
   - If current start ≤ previous merged end, merge them.
   - Otherwise push as a new interval.
4. Return merged intervals.

Because sorting groups overlapping intervals together, a single linear scan is enough after sorting.

---

# Complexity

| Approach | Time | Space |
|-----------|------|--------|
| Brute Force | O(N²) | O(N) |
| Optimal | O(N log N) + O(N) | O(N) |