# 162. Find Peak Element

## Problem Statement

A **peak element** is an element that is strictly greater than its neighbors.

Given an integer array `nums`, find a peak element and return its index.

You may assume:

```cpp
nums[-1] = nums[n] = -∞
```

If multiple peaks exist, return the index of any peak.

---

# Brute Force Approach

## Idea

Traverse the array and check every element.

If an element is greater than both neighbors, it is a peak.

---

## Code

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {

        int n = nums.size();

        for(int i = 0; i < n; i++) {

            bool left =
                (i == 0 || nums[i] > nums[i - 1]);

            bool right =
                (i == n - 1 || nums[i] > nums[i + 1]);

            if(left && right)
                return i;
        }

        return -1;
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

We are **not asked to find the maximum element**.

We only need **any peak element**.

---

A peak always exists because:

```cpp
nums[-1] = nums[n] = -∞
```

---

# Important Observation

At any index:

### Case 1: Increasing Slope

```text
1 2 3 4 5
      ^
     mid
```

If:

```cpp
nums[mid] < nums[mid + 1]
```

Then a peak must exist on the right side.

Why?

Because:

- Array keeps increasing → last element becomes peak.
- Or it increases and then decreases → peak formed somewhere.

So:

```cpp
low = mid + 1;
```

---

### Case 2: Decreasing Slope

```text
5 4 3 2 1
    ^
   mid
```

If:

```cpp
nums[mid] > nums[mid + 1]
```

Then a peak exists on the left side (including mid).

Why?

Because:

- We are already on a descending slope.
- A peak must exist before or at mid.

So:

```cpp
high = mid;
```

---

# Why Binary Search Works

Instead of checking whether `mid` is peak directly,

we check the direction:

```cpp
nums[mid] < nums[mid + 1]
```

Move right.

Else:

```cpp
move left.
```

Eventually both pointers meet at a peak.

---

# Optimal Code

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {

        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {

            int mid = low + (high - low) / 2;

            if(nums[mid] < nums[mid + 1]) {
                low = mid + 1;
            }
            else {
                high = mid;
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
| Time | O(log n) |
| Space | O(1) |

---

# Key Takeaway

For LC 162, don't search for the maximum element.

Search for the **direction of the slope**:

```cpp
Increasing  -> go right
Decreasing  -> go left
```

This guarantees reaching a peak in **O(log n)** time.