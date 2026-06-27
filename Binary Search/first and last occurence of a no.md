# LeetCode 34: Find First and Last Position of Element in Sorted Array

## Problem Statement

Given a sorted array `nums` in non-decreasing order and a target value `target`, return the starting and ending position of the target.

If the target is not found, return `[-1, -1]`.

### Example

```cpp
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

---

# Intuition

Since the array is **sorted**, Binary Search can be used.

The target may appear multiple times, so instead of stopping when we find it, we need:

1. First occurrence (leftmost index)
2. Last occurrence (rightmost index)

Thus, perform Binary Search twice:

- First Binary Search → Find first occurrence.
- Second Binary Search → Find last occurrence.

---

# Why Binary Search Works?

In a sorted array:

- Elements on the left are smaller.
- Elements on the right are larger.

Whenever we find the target:

- For first occurrence, continue searching on the left.
- For last occurrence, continue searching on the right.

This ensures we reach the extreme positions.

---

# Approach 1: Separate Binary Searches

## Finding First Occurrence

Whenever:

```cpp
nums[mid] == target
```

Store:

```cpp
ans = mid;
```

But continue searching left:

```cpp
high = mid - 1;
```

Because there may be another occurrence before it.

---

## Finding Last Occurrence

Whenever:

```cpp
nums[mid] == target
```

Store:

```cpp
ans = mid;
```

But continue searching right:

```cpp
low = mid + 1;
```

Because there may be another occurrence after it.

---

# Code (Optimal)

```cpp
class Solution {
public:

    int firstOccurrence(vector<int>& nums, int target) {
        int low = 0, high = nums.size() - 1;
        int ans = -1;

        while(low <= high) {
            int mid = low + (high - low) / 2;

            if(nums[mid] == target) {
                ans = mid;
                high = mid - 1;
            }
            else if(nums[mid] < target) {
                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }

        return ans;
    }

    int lastOccurrence(vector<int>& nums, int target) {
        int low = 0, high = nums.size() - 1;
        int ans = -1;

        while(low <= high) {
            int mid = low + (high - low) / 2;

            if(nums[mid] == target) {
                ans = mid;
                low = mid + 1;
            }
            else if(nums[mid] < target) {
                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }

        return ans;
    }

    vector<int> searchRange(vector<int>& nums, int target) {

        int first = firstOccurrence(nums, target);

        if(first == -1)
            return {-1, -1};

        int last = lastOccurrence(nums, target);

        return {first, last};
    }
};
```

---

# Alternative STL Solution

Using:

```cpp
lower_bound()
upper_bound()
```

### Idea

- `lower_bound()` → First position where target can be inserted.
- `upper_bound()` → First position greater than target.

Therefore:

```cpp
first = lower_bound(...)
last = upper_bound(...) - 1
```

---

## STL Code

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {

        auto first = lower_bound(nums.begin(), nums.end(), target);

        if(first == nums.end() || *first != target)
            return {-1, -1};

        auto last = upper_bound(nums.begin(), nums.end(), target);

        return {
            (int)(first - nums.begin()),
            (int)(last - nums.begin() - 1)
        };
    }
};
```

---

### Time Complexity

```cpp
lower_bound()  -> O(log n)
upper_bound()  -> O(log n)
```

Total:

```cpp
O(log n)
```

---

# Space Complexity

For both approaches:

```cpp
O(1)
```

No extra data structures are used.

---

# Extension: Count Occurrences of Target in Sorted Array

This is a direct follow-up to LeetCode 34.

Once we know the:

- First Occurrence
- Last Occurrence

we can easily find how many times the target appears.

---

# Problem Statement

Given a sorted array and a target value, count the number of times the target occurs.

### Example

```cpp
nums = [1,2,2,2,3,4]
target = 2

Output = 3
```

---

# Intuition

If:

```cpp
first = index of first occurrence
last  = index of last occurrence
```

Then all occurrences lie between them.

Therefore:

```cpp
count = last - first + 1
```

---

# Code

```cpp
int firstOccurrence(vector<int>& nums, int target) {
    int low = 0, high = nums.size() - 1;
    int ans = -1;

    while(low <= high) {
        int mid = low + (high - low) / 2;

        if(nums[mid] == target) {
            ans = mid;
            high = mid - 1;
        }
        else if(nums[mid] < target) {
            low = mid + 1;
        }
        else {
            high = mid - 1;
        }
    }

    return ans;
}

int lastOccurrence(vector<int>& nums, int target) {
    int low = 0, high = nums.size() - 1;
    int ans = -1;

    while(low <= high) {
        int mid = low + (high - low) / 2;

        if(nums[mid] == target) {
            ans = mid;
            low = mid + 1;
        }
        else if(nums[mid] < target) {
            low = mid + 1;
        }
        else {
            high = mid - 1;
        }
    }

    return ans;
}

int countOccurrences(vector<int>& nums, int target) {

    int first = firstOccurrence(nums, target);

    if(first == -1)
        return 0;

    int last = lastOccurrence(nums, target);

    return last - first + 1;
}
```

---

# Time Complexity

### Binary Search Approach

```cpp
First Occurrence = O(log n)
Last Occurrence  = O(log n)
```

Total:

```cpp
O(log n)
```

---

# Space Complexity

```cpp
O(1)
```

---