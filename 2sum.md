# Two Sum (LeetCode 1)

## Problem Statement

Given an array of integers `nums` and an integer `target`, return the indices of the two numbers such that they add up to `target`.

---

# Approach 1: Brute Force

## Idea

Check every possible pair of elements.

For each element, compare it with every element after it and see if their sum equals the target.

## Algorithm

1. Traverse the array using index `i`.
2. For each `i`, traverse all elements after it using index `j`.
3. If `nums[i] + nums[j] == target`, return `{i, j}`.

## Code

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();

        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j < n; j++) {
                if(nums[i] + nums[j] == target) {
                    return {i, j};
                }
            }
        }

        return {};
    }
};
```

## Complexity Analysis

### Time Complexity

```text
O(n²)
```

Because every pair is checked.

### Space Complexity

```text
O(1)
```

No extra space is used.

---

# Approach 2: Hash Map (Optimal)

## Key Observation

For every number `x`, we need another number:

```cpp
target - x
```

Instead of searching for it every time, store previously seen numbers in a hash map.

---

## Idea

Maintain a hash map:

```cpp
number -> index
```

For each element:

1. Calculate its complement.
2. Check if the complement already exists in the map.
3. If yes, return the stored index and current index.
4. Otherwise, store the current number and index.

---

## Optimal Code

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> mp;

        for(int i = 0; i < nums.size(); i++) {

            int complement = target - nums[i];

            if(mp.find(complement) != mp.end()) {
                return {mp[complement], i};
            }

            mp[nums[i]] = i;
        }

        return {};
    }
};
```

---

## Complexity Analysis

### Time Complexity

```text
O(n)
```

Each hash map operation (`find` and `insert`) takes approximately O(1).

### Space Complexity

```text
O(n)
```

The hash map may store all elements.

---

# Interview Explanation

### Brute Force

Use two nested loops and check every possible pair. If the sum equals the target, return the indices.

- Time: O(n²)
- Space: O(1)

### Optimal

Use a hash map to store previously seen numbers and their indices. For every element, calculate the complement (`target - nums[i]`). If the complement exists in the map, return both indices; otherwise insert the current element.

- Time: O(n)
- Space: O(n)

---

# Pattern Learned

This problem introduces the **Hash Map Lookup Pattern**.

Whenever you see:

- Find a pair
- Find a complement
- Two numbers sum to target
- Fast lookups required

Think:

```cpp
unordered_map
```
---
