# 1539. Kth Missing Positive Number

## Problem Statement

Given a sorted array of positive integers:

```cpp
arr[]
```

and an integer:

```cpp
k
```

Return the **k-th missing positive number**.

---

## Example 1

```cpp
Input:
arr = [2,3,4,7,11]
k = 5

Output:
9
```

### Explanation

Missing numbers:

```text
1, 5, 6, 8, 9, 10, ...
```

5th missing number:

```cpp
9
```

---

# Brute Force Approach

## Intuition

Whenever we encounter a number:

```cpp
arr[i]
```

that is less than or equal to `k`, it means one missing number gets shifted further ahead.

Therefore:

```cpp
k++
```

Continue until an element becomes larger than `k`.

The final value of `k` becomes the answer.

---

## Why Does This Work?

Example:

```cpp
arr = [2,3,4,7,11]
k = 5
```

Initially:

```cpp
Need 5th missing number
```

---

### arr[0] = 2

```cpp
2 <= 5
```

Increase:

```cpp
k = 6
```

---

### arr[1] = 3

```cpp
3 <= 6
```

Increase:

```cpp
k = 7
```

---

### arr[2] = 4

```cpp
4 <= 7
```

Increase:

```cpp
k = 8
```

---

### arr[3] = 7

```cpp
7 <= 8
```

Increase:

```cpp
k = 9
```

---

### arr[4] = 11

```cpp
11 > 9
```

Stop.

Answer:

```cpp
9
```

---

## Code

```cpp
class Solution {
public:
    int findKthPositive(vector<int>& arr, int k) {

        for(int i = 0; i < arr.size(); i++) {

            if(arr[i] <= k) {
                k++;
            }
            else {
                break;
            }
        }

        return k;
    }
};
```

---

## Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(n) |
| Space | O(1) |

---

# Optimal Approach (Binary Search)

## Key Observation

Instead of finding missing numbers one by one,

let's calculate:

```cpp
How many numbers are missing till index i?
```

---

# Formula Derivation

Consider:

```cpp
arr = [2,3,4,7,11]
```

Indexes:

```text
Index : 0 1 2 3 4
Value : 2 3 4 7 11
```

---

### At index 0

Value:

```cpp
2
```

Numbers expected till 2:

```text
1,2
```

Count:

```cpp
2
```

Actual elements present:

```cpp
1 element
```

Missing:

```cpp
2 - 1 = 1
```

Formula:

```cpp
arr[i] - (i + 1)
```

---

### At index 3

Value:

```cpp
7
```

Numbers expected till 7:

```text
1,2,3,4,5,6,7
```

Count:

```cpp
7
```

Actual elements present:

```cpp
4
```

Missing:

```cpp
7 - 4 = 3
```

Formula:

```cpp
7 - (3+1)
= 3
```

---

# General Formula

Missing numbers before index `i`:

```cpp
missing = arr[i] - (i + 1)
```

OR

```cpp
missing = arr[i] - i - 1
```

This is the most important observation.

---

# Example

```cpp
arr = [2,3,4,7,11]
```

| Index | Value | Missing Count |
|---------|---------|---------|
| 0 | 2 | 1 |
| 1 | 3 | 1 |
| 2 | 4 | 1 |
| 3 | 7 | 3 |
| 4 | 11 | 6 |

---

Notice:

```text
1, 1, 1, 3, 6
```

Missing count is increasing.

Monotonic.

Therefore:

```cpp
Binary Search
```

can be applied.

---

# What Are We Searching?

We need the first index where:

```cpp
missing >= k
```

Example:

```cpp
k = 5
```

Missing counts:

```text
1 1 1 3 6
        ^
```

First position where:

```cpp
missing >= 5
```

is:

```cpp
index = 4
```

---

# Binary Search

If:

```cpp
missing < k
```

Need more missing numbers.

Go right.

```cpp
low = mid + 1
```

---

If:

```cpp
missing >= k
```

Possible answer.

Go left.

```cpp
high = mid - 1
```

---

### Meaning

At index:

```cpp
high = 3
```

Missing count:

```cpp
3
```

Still less than:

```cpp
k = 5
```

---

Need:

```cpp
5 - 3 = 2
```

more missing numbers after `arr[3]`.

---

Current value:

```cpp
arr[3] = 7
```

Move 2 more positions:

```cpp
7 + 2 = 9
```

Answer:

```cpp
9
```

---

# Final Formula Derivation

After Binary Search:

```cpp
high = last index having missing < k
```

Missing till high:

```cpp
arr[high] - (high + 1)
```

Remaining:

```cpp
k - missing
```

Answer:

```cpp
arr[high] + (k - missing)
```

---

## Simplified Formula

After BS ends:

```cpp
Answer = low + k
```

---

### Why?

At the end:

```cpp
low = first index where missing >= k
```

Before that:

```cpp
low elements are present
```

To obtain the kth missing number:

```cpp
k missing numbers
+ low existing numbers
```

Hence:

```cpp
Answer = low + k
```

---

# Optimal Code

```cpp
class Solution {
public:
    int findKthPositive(vector<int>& arr, int k) {

        int low = 0;
        int high = arr.size() - 1;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            int missing = arr[mid] - (mid + 1);

            if(missing < k) {
                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }

        return low + k;
    }
};
```

---

# Why Return `low + k`?

At the end:

```cpp
low
```

represents how many array elements exist before the kth missing number.

Therefore:

```cpp
k missing numbers + low present numbers
```

gives the actual value.

Hence:

```cpp
Answer = low + k
```

---

# Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(log n) |
| Space | O(1) |