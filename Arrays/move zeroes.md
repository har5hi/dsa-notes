# LeetCode 283 - Move Zeroes

## Problem Statement

Given an integer array `nums`, move all `0`s to the end of the array while maintaining the relative order of the non-zero elements.

The operation must be performed in-place without making a copy of the array.

---

## Approach

We use the Two Pointer approach:

* `i` points to the position where the next non-zero element should be placed.
* `j` traverses the array.

For every element:

* If `nums[j]` is non-zero, swap it with `nums[i]`.
* Increment `i` to point to the next available position.

This ensures:

* All non-zero elements are moved to the front.
* Their relative order remains unchanged.
* All zeros automatically shift towards the end.

---

## Intuition

Think of `i` as the boundary between:

* Processed non-zero elements.
* Remaining elements.

Whenever a non-zero element is found, place it at the next available position (`i`) and expand the boundary.

Example:

```cpp
[0,1,0,3,12]
```

After processing:

```cpp
[1,3,12,0,0]
```

---

## Code

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size();
        int i = 0;

        for(int j = 0; j < n; j++) {
            if(nums[j] != 0) {
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                i++;
            }
        }
    }
};
```

---

## Time Complexity

* Time Complexity: O(n)
* Space Complexity: O(1)

The array is traversed only once and no extra data structure is used.

---