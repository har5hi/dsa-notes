# LeetCode 136 - Single Number

## Problem Statement

Given a non-empty array of integers `nums`, every element appears **twice** except for one element that appears only **once**.

Find and return that single element.

You must solve it with:

* Linear runtime complexity.
* Constant extra space.

---

## Approach

The key observation is the property of the **XOR (`^`) operator**:

* `a ^ a = 0`
* `a ^ 0 = a`
* XOR is commutative and associative.

This means that when we XOR all elements together:

* Every number that appears twice cancels itself out.
* Only the number appearing once remains.

Example:

```text
nums = [4,1,2,1,2]

4 ^ 1 ^ 2 ^ 1 ^ 2

= 4 ^ (1 ^ 1) ^ (2 ^ 2)

= 4 ^ 0 ^ 0

= 4
```

So we simply XOR all elements and return the result.

---

## Mistakes I Made While Solving

### 1. Thinking About Sorting First

My first thought was to sort the array and then check adjacent elements.

Example:

```cpp
sort(nums.begin(), nums.end());
```

Although this works, sorting takes:

```text
O(n log n)
```

The problem specifically asks for linear time complexity, so sorting is not optimal.

---

### 2. Using a Hash Map

I also considered storing frequencies:

```cpp
unordered_map<int,int> mp;
```

Then finding the element with frequency `1`.

This works but requires:

```text
O(n) extra space
```

The problem asks for constant extra space, so this approach does not satisfy the constraints.

---

## Code

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        
        int xorr = 0;
        for(int i = 0; i<nums.size(); i++){
            xorr = xorr^nums[i];
        }
        return xorr;
    }
};
```

---

## Time Complexity

* Time Complexity: O(n)
* Space Complexity: O(1)

The array is traversed only once, and only a single variable is used for computation.

--- 