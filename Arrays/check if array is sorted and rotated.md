# LeetCode 1752 - Check if Array Is Sorted and Rotated

## Problem Statement

Given an array `nums`, return `true` if the array was originally sorted in non-decreasing order and then rotated some number of positions (including zero). Otherwise, return `false`.

---

## Key Observation

A sorted array has elements in non-decreasing order:

```cpp
[1,2,3,4,5]
```

If the array is rotated:

```cpp
[3,4,5,1,2]
```

there will be exactly one position where the order decreases:

```cpp
5 > 1
```

We call this a **break** 

A valid sorted and rotated array can have:

* 0 breaks (already sorted)
* 1 break (sorted and rotated)

If there are more than 1 break, the array is invalid.

---

## Approach

Traverse the array and count the number of times:

```cpp
nums[i] > nums[(i + 1) % n]
```

If the count exceeds 1, return `false`.

Otherwise, return `true`.

The modulo operator `%` helps compare the last element with the first element, treating the array as circular.

---

## Code: 

```cpp
class Solution {
public:
    bool check(vector<int>& nums) {

        int count = 0;
        int n = nums.size();

        for(int i = 0; i < n; i++) {

            if(nums[i] > nums[(i + 1) % n]) {
                count++;
            }

            if(count > 1)
                return false;
        }

        return true;
    }
};
```

---

## Mistakes I Made While Solving

### 1. Initially Checked Only if the Array Was Sorted

I first tried solving:

```cpp
if(arr[i] < arr[i-1])
```

This works for checking a sorted array but fails for rotated arrays.

---

### 2. Forgot to Compare the Last Element with the First

Incorrect:

```cpp
for(int i = 0; i < n-1; i++)
{
    if(nums[i] > nums[i+1])
        count++;
}
```

Issue:

The comparison:

```cpp
nums[n-1] vs nums[0]
```

is never checked.

Correct:

```cpp
nums[i] > nums[(i+1)%n]
```

---

### 3. Understanding the Modulo Operator

The most important line:

```cpp
nums[(i + 1) % n]
```

Example:

```cpp
n = 5
i = 4

(4 + 1) % 5
= 5 % 5
= 0
```

So the last element is compared with the first element.

This allows us to treat the array as circular.

---

## Time Complexity

```text
O(n)
```

Only one traversal of the array.

---

## Space Complexity

```text
O(1)
```

Only a few variables are used.