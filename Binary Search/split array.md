# Painter's Partition Problem

## Problem Statement

Given:

```cpp
boards[]
```

where:

```cpp
boards[i]
```

represents the length of the `i-th` board.

And:

```cpp
k painters
```

Each painter:

- Paints contiguous boards only.
- Cannot split a board.
- Each board must be painted exactly once.

Return the **minimum time required to paint all boards** assuming:

```cpp
1 unit board = 1 unit time
```

---

## Example

```cpp
boards = [10,20,30,40]
k = 2
```

Output:

```cpp
60
```

---

### Optimal Partition

```text
Painter 1 → 10 + 20 + 30 = 60

Painter 2 → 40
```

Maximum time:

```cpp
60
```

Minimum possible answer.

---

# Observation

This problem is identical to:

```text
Book Allocation Problem
```

Replace:

```text
Books → Boards
Students → Painters
Pages → Time
```

Everything else remains exactly the same.

---

# Brute Force Approach

Search answer from:

```cpp
max(boards)
```

to

```cpp
sum(boards)
```

For each value:

```cpp
timeLimit
```

calculate painters required.

Return first valid answer.

---

# Helper Function

For a given time limit:

```cpp
mid
```

count painters required.

---

## Greedy Strategy

Keep assigning boards.

If:

```cpp
current + board > limit
```

assign a new painter.

---

# Binary Search Observation

Suppose:

```cpp
time = 80
```

works.

Then:

```cpp
81, 82, 83 ...
```

will also work.

---

Suppose:

```cpp
time = 50
```

does not work.

Then:

```cpp
49, 48, 47 ...
```

will never work.

---

Monotonic pattern:

```text
Time

40 50 60 70 80
✗  ✗  ✓  ✓  ✓
```

Need:

```text
First Valid Answer
```

Hence Binary Search.

---

# Optimal Code

```cpp
class Solution {
public:

    int countPainters(vector<int>& boards, int limit) {

        int painters = 1;
        long long sum = 0;

        for(int board : boards) {

            if(sum + board <= limit) {
                sum += board;
            }
            else {
                painters++;
                sum = board;
            }
        }
        return painters;
    }

    int paintersPartition(vector<int>& boards, int k) {

        int low = *max_element(boards.begin(), boards.end());

        int high = accumulate(boards.begin(), boards.end(), 0);

        while(low <= high) {

            int mid = low + (high - low) / 2;

            int painters = countPainters(boards, mid);

            if(painters <= k) {
                high = mid - 1;
            }
            else {
                low = mid + 1;
            }
        }
        return low;
    }
};
```

---

# Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(n × log(sum)) |
| Space | O(1) |

---
# 410. Split Array Largest Sum

## Problem Statement

Given:

```cpp
nums[]
```

and an integer:

```cpp
k
```

Split the array into exactly:

```cpp
k non-empty subarrays
```

Return the minimum possible value of:

```cpp
largest subarray sum
```

---

## Example

```cpp
nums = [7,2,5,10,8]
k = 2
```

Output:

```cpp
18
```

---

### Explanation

Split:

```text
[7,2,5]  [10,8]
```

Subarray sums:

```cpp
14
18
```

Largest sum:

```cpp
18
```

Minimum possible.

---

# Optimal Code

```cpp
class Solution {
public:

    int countSubarrays(vector<int>& nums, int limit) {

        int subarrays = 1;
        long long sum = 0;

        for(int num : nums) {

            if(sum + num <= limit) { 
                sum += num;
            }
            else {
                subarrays++;
                sum = num;
            }
        }
        return subarrays;
    }

    int splitArray(vector<int>& nums, int k) {

        int low = *max_element(nums.begin(), nums.end());

        int high = accumulate(nums.begin(), nums.end(), 0);

        while(low <= high) {

            int mid = low + (high - low) / 2;

            int parts = countSubarrays(nums, mid);

            if(parts <= k) {
                high = mid - 1;
            }
            else {
                low = mid + 1;
            }
        }
        return low;
    }
};
```

---

# Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(n × log(sum(nums))) |
| Space | O(1) |

---

# Relationship Between These Problems

| Problem | Resource | Groups |
|----------|----------|---------|
| Book Allocation | Pages | Students |
| Painter's Partition | Boards | Painters |
| Split Array Largest Sum (LC 410) | Elements | Subarrays |

All three use:

```text
Binary Search on Answer
```

with the exact same pattern:

```cpp
low  = maximum element
high = sum of all elements
```

and

```cpp
if(groupsNeeded <= k)
    high = mid - 1;
else
    low = mid + 1;
```