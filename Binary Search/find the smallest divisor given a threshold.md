# 1283. Find the Smallest Divisor Given a Threshold

## Problem Statement

Given an array:

```cpp
nums[]
```

and an integer:

```cpp
threshold
```

Choose a positive integer divisor `d`.

For every element:

```cpp
nums[i]
```

compute:

```cpp
ceil(nums[i] / d)
```

and take their sum.

Return the **smallest divisor** such that:

```cpp
Σ ceil(nums[i] / d) <= threshold
```

---

# Brute Force Approach

## Idea

Try every divisor from:

```cpp
1 → max(nums)
```

For each divisor:

- Calculate total sum.
- Return first divisor satisfying:

```cpp
sum <= threshold
```

---

## Code

```cpp
int smallestDivisor(vector<int>& nums,
                    int threshold) {

    int maxi =
        *max_element(nums.begin(),
                     nums.end());

    for(int d = 1; d <= maxi; d++) {

        int sum = 0;

        for(int num : nums) {
            sum += ceil((double)num / d);
        }

        if(sum <= threshold)
            return d;
    }

    return -1;
}
```

---

## Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(max(nums) × n) |
| Space | O(1) |

Too slow.

---

# Optimal Approach (Binary Search on Answer)

## Key Observation

Suppose:

```cpp
nums = [1,2,5,9]
```

Check different divisors:

---

Notice:

```text
Divisor ↑
Sum ↓
```

As divisor increases:

```cpp
ceil(nums[i]/divisor)
```

decreases. This forms a monotonic pattern.

---

# Search Space

Smallest divisor:

```cpp
1
```

Largest divisor:

```cpp
max(nums)
```

Because larger values don't provide any additional benefit.

```cpp
low = 1
high = max(nums)
```

---

# Helper Function

For a divisor:

```cpp
d
```

Compute:

```cpp
Σ ceil(nums[i] / d)
```

---

## Efficient Ceiling Formula

Instead of:

```cpp
ceil((double)num / d)
```

Use:

```cpp
(num + d - 1) / d
```

Example:

```cpp
num = 9
d = 5

(9 + 5 - 1)/5
= 13/5
= 2
```

---

# Binary Search Logic

### If

```cpp
sum <= threshold
```

Current divisor works. Try smaller divisor.

```cpp
high = mid - 1
```

---

### If

```cpp
sum > threshold
```

Current divisor too small. Need larger divisor.

```cpp
low = mid + 1
```

---

# Optimal Code

```cpp
class Solution {
public:

    int calculateSum(vector<int>& nums,
                     int divisor) {

        int sum = 0;

        for(int num : nums) {

            sum +=
            (num + divisor - 1) / divisor;
        }

        return sum;
    }

    int smallestDivisor(vector<int>& nums,
                        int threshold) {

        int low = 1;

        int high =
            *max_element(nums.begin(),
                         nums.end());

        while(low <= high) {

            int mid =
                low + (high - low) / 2;

            int sum =
                calculateSum(nums, mid);

            if(sum <= threshold) {

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

Let:

```cpp
n = nums.size()
m = max(nums)
```

### Binary Search

```cpp
O(log m)
```

### Each Check

```cpp
O(n)
```

### Total

| Complexity | Value |
|------------|--------|
| Time | O(n × log(max(nums))) |
| Space | O(1) |

---