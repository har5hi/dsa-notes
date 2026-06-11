# LeetCode 26 - Remove Duplicates from Sorted Array

## Problem Statement

Given a sorted integer array `nums`, remove the duplicates in-place such that each unique element appears only once.

Return the number of unique elements `k`.

The first `k` elements of `nums` should contain the unique elements in their original order.

---

## Approach

Since the array is already sorted, duplicate elements will always appear consecutively.

We use the Two Pointer approach:

* `i` points to the last unique element found.
* `j` traverses the array.

For every element:

* If `nums[j]` is different from `nums[i]`, a new unique element is found.
* Place it at position `i + 1`.
* Increment `i`.

Finally, return `i + 1`, which represents the count of unique elements.

---

## Mistakes I Made While Solving

### 1. Off-by-One Error in Loop Condition

I initially wrote:

```cpp
for(int j = 1; j <= nums.size(); j++)
```

This causes `j` to reach `nums.size()`, which is an invalid index.

Accessing `nums[3]` results in undefined behavior and may produce garbage values.

Correct:

```cpp
for(int j = 1; j < nums.size(); j++)
```

---

## Code

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int i = 0;

        for(int j = 1; j < nums.size(); j++) {
            if(nums[j] != nums[i]) {
                nums[i + 1] = nums[j];
                i++;
            }
        }

        return i + 1;
    }
};
```

---

## Time Complexity

* Time Complexity: O(n)
* Space Complexity: O(1)

The array is traversed only once and no extra space is used.

---

## Key Learning

Whenever iterating through an array:

```cpp
for(int i = 0; i < n; i++)
```

Always remember:

* Last valid index = `n - 1`
* Using `<= n` usually leads to out-of-bounds access.
* Most array traversal bugs come from off-by-one errors.
