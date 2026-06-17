# 53. Maximum Subarray

## Problem Statement

Given an integer array `nums`, find the contiguous subarray with the largest sum and return that sum.

### Example

```cpp
nums = [-2,1,-3,4,-1,2,1,-5,4]
```

Output:

```cpp
[4,-1,2,1]
```

has the largest sum:

```cpp
4 + (-1) + 2 + 1 = 6
```

---

# Approach 1: Brute Force (O(n³))

Generate every possible subarray and calculate its sum.

### Idea

* Choose starting index `i`
* Choose ending index `j`
* Calculate sum of elements from `i` to `j`
* Update maximum sum

### Code

```cpp
int maxSubArray(vector<int>& nums) {
    int maxi = INT_MIN;

    for(int i = 0; i < nums.size(); i++) {
        for(int j = i; j < nums.size(); j++) {

            int sum = 0;

            for(int k = i; k <= j; k++) {
                sum += nums[k];
            }

            maxi = max(maxi, sum);
        }
    }

    return maxi;
}
```

### Complexity

* Time: `O(n³)`
* Space: `O(1)`

---

# Approach 2: Better Solution (O(n²))

Instead of recalculating subarray sums repeatedly, keep extending the current subarray.

### Idea

For every starting index:

* Initialize `sum = 0`
* Extend subarray one element at a time
* Add current element to running sum
* Update answer

### Code

```cpp
int maxSubArray(vector<int>& nums) {
    int maxi = INT_MIN;

    for(int i = 0; i < nums.size(); i++) {

        int sum = 0;

        for(int j = i; j < nums.size(); j++) {

            sum += nums[j];

            maxi = max(maxi, sum);
        }
    }

    return maxi;
}
```

### Complexity

* Time: `O(n²)`
* Space: `O(1)`

---

# Approach 3: Kadane's Algorithm (Optimal)

### Observation

If the current subarray sum becomes harmful (negative), carrying it forward will only decrease future sums.

So at every index:

* Either start a new subarray
* Or extend the previous subarray

Take whichever gives a larger sum.

---

## Key Idea

For every element:

```cpp
currentSum = max(nums[i], currentSum + nums[i]);
```

Two choices:

### Start New Subarray

```cpp
nums[i]
```

### Extend Previous Subarray

```cpp
currentSum + nums[i]
```

Choose the better option.

---

# Kadane's Algorithm Code

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {

        int currentSum = nums[0];
        int maxSum = nums[0];

        for(int i = 1; i < nums.size(); i++) {

            currentSum = max(nums[i],
                             currentSum + nums[i]);

            maxSum = max(maxSum, currentSum);
        }

        return maxSum;
    }
};
```

---

# Dry Run

Input:

```cpp
[-2,1,-3,4,-1,2,1,-5,4]
```

Initial:

```cpp
currentSum = -2
maxSum = -2
```

| i | nums[i] | currentSum    | maxSum |
| - | ------- | ------------- | ------ |
| 0 | -2      | -2            | -2     |
| 1 | 1       | max(1,-1)=1   | 1      |
| 2 | -3      | max(-3,-2)=-2 | 1      |
| 3 | 4       | max(4,2)=4    | 4      |
| 4 | -1      | max(-1,3)=3   | 4      |
| 5 | 2       | max(2,5)=5    | 5      |
| 6 | 1       | max(1,6)=6    | 6      |
| 7 | -5      | max(-5,1)=1   | 6      |
| 8 | 4       | max(4,5)=5    | 6      |

Final Answer:

```cpp
6
```

Subarray:

```cpp
[4,-1,2,1]
```

---

# Why Kadane Works

Suppose:

```cpp
currentSum = -10
```

Next element:

```cpp
5
```

Extending:

```cpp
-10 + 5 = -5
```

Starting fresh:

```cpp
5
```

Clearly starting fresh is better.

That's why Kadane uses:

```cpp
currentSum = max(nums[i], currentSum + nums[i]);
```

and automatically discards bad prefixes.

---

# Important Interview Points

### What does `currentSum` represent?

Maximum subarray sum ending at the current index.

### What does `maxSum` represent?

Maximum subarray sum found so far.

### Why initialize with `nums[0]`?

To correctly handle all-negative arrays.

Example:

```cpp
[-4,-2,-7]
```

Answer:

```cpp
-2
```

Using `0` would give the wrong answer.

---

# Complexity Analysis

| Approach           | Time  | Space |
| ------------------ | ----- | ----- |
| Brute Force        | O(n³) | O(1)  |
| Better             | O(n²) | O(1)  |
| Kadane's Algorithm | O(n)  | O(1)  |

---

# Takeaway

Kadane's Algorithm works by deciding at every index:

* Start a new subarray from the current element
* OR continue the previous subarray

```cpp
currentSum = max(nums[i], currentSum + nums[i]);
```

This greedy decision guarantees the maximum subarray sum in a single traversal.