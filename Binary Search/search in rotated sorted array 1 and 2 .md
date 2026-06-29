# 33. Search in Rotated Sorted Array

## Problem Statement
Given a rotated sorted array `nums` with **distinct** integers and a target value, return the index of the target if it exists, otherwise return `-1`.

### Example
```cpp
nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

---

# Intuition

A normal sorted array allows Binary Search because one half is always sorted.

In a rotated sorted array:

```text
[4,5,6,7,0,1,2]
```

At any point during Binary Search:

- At least one half will always be sorted.
- Identify the sorted half.
- Check if target lies inside that sorted range.
- If yes, move there.
- Otherwise search the other half.

This keeps eliminating half of the array every time.

---

# Observation

For any mid:

### Left Half Sorted

```cpp
nums[low] <= nums[mid]
```

Example:

```text
4 5 6 | 7 | 0 1 2
^     ^
low   mid
```

If target lies between:

```cpp
nums[low] <= target < nums[mid]
```

search left.

Else search right.

---

### Right Half Sorted

Otherwise:

```cpp
nums[mid] <= nums[high]
```

Example:

```text
4 5 6 7 | 0 | 1 2
          ^
         mid
```

If target lies between:

```cpp
nums[mid] < target <= nums[high]
```

search right.

Else search left.

---

# Algorithm

1. Initialize `low = 0`, `high = n-1`
2. Find `mid`
3. If target found, return index.
4. Check which half is sorted.
5. Determine whether target belongs to that half.
6. Eliminate the other half.
7. Repeat until search space becomes empty.

---

# Code

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {

        int low = 0;
        int high = nums.size() - 1;

        while(low <= high){

            int mid = low + (high - low) / 2;

            if(nums[mid] == target)
                return mid;

            // Left half sorted
            if(nums[low] <= nums[mid]){

                if(nums[low] <= target && target < nums[mid])
                    high = mid - 1;
                else
                    low = mid + 1;
            }

            // Right half sorted
            else{

                if(nums[mid] < target && target <= nums[high])
                    low = mid + 1;
                else
                    high = mid - 1;
            }
        }

        return -1;
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

# Why It Works

At every step:

- One side is guaranteed to be sorted.
- We can determine whether the target belongs to that sorted side.
- Half of the search space is discarded each iteration.

This is exactly why Binary Search still works even after rotation.

---

---
# 81. Search in Rotated Sorted Array II

## Difference from Part I

Now duplicates are allowed.

Example:

```cpp
nums = [2,5,6,0,0,1,2]
target = 0
```

Output:

```cpp
true
```

---

# Problem with Duplicates

Consider:

```text
1 0 1 1 1
```

```cpp
low = 0
mid = 2
high = 4
```

Values:

```text
1 1 1
```

We cannot determine:

- Left sorted?
- Right sorted?

Both appear identical.

Binary Search loses information.

---

# Key Observation

When:

```cpp
nums[low] == nums[mid] &&
nums[mid] == nums[high]
```

We cannot identify the sorted side.

So simply shrink search space:

```cpp
low++;
high--;
```

and continue.

---

# Algorithm

1. Find mid.
2. If target found → return true.
3. If low, mid, high all equal:
   - remove duplicates.
4. Otherwise identify sorted half.
5. Search the correct side.
6. Continue.

---

# Code

```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {

        int low = 0;
        int high = nums.size() - 1;

        while(low <= high){

            int mid = low + (high - low) / 2;

            if(nums[mid] == target)
                return true;

            // duplicates
            if(nums[low] == nums[mid] &&
               nums[mid] == nums[high]){

                low++;
                high--;
                continue;
            }

            // left sorted
            if(nums[low] <= nums[mid]){

                if(nums[low] <= target &&
                   target < nums[mid])
                    high = mid - 1;
                else
                    low = mid + 1;
            }

            // right sorted
            else{

                if(nums[mid] < target &&
                   target <= nums[high])
                    low = mid + 1;
                else
                    high = mid - 1;
            }
        }

        return false;
    }
};
```

---

# Complexity Analysis

### Average Case

| Complexity | Value |
|------------|--------|
| Time | O(log n) |
| Space | O(1) |

### Worst Case

When many duplicates exist:

```text
[1,1,1,1,1,1,1]
```

We keep shrinking by one from both ends.

| Complexity | Value |
|------------|--------|
| Time | O(n) |
| Space | O(1) |

---

# Why Part II Can Become O(n)?

Duplicates can hide the sorted half.

Example:

```text
1 1 1 1 1 1 1
```

Every iteration:

```cpp
low++;
high--;
```

Only a few elements are removed.
Hence worst-case complexity degrades from:

```text
O(log n) → O(n)
```

---