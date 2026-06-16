# Majority Element (LeetCode 169)

## Problem Statement

Given an array `nums` of size `n`, return the **majority element**.

The majority element is the element that appears **more than ⌊n/2⌋ times**.

You may assume that the majority element always exists in the array.

### Example

```cpp
Input: nums = [2,2,1,1,1,2,2]
Output: 2
```

---

# Approach 1: Brute Force

For every element, count its frequency by traversing the entire array.

If any element appears more than `n/2` times, return it.

## Algorithm

1. Traverse the array.
2. For each element, count its occurrences.
3. If frequency > `n/2`, return that element.

## Code

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size();

        for(int i = 0; i < n; i++) {
            int count = 0;

            for(int j = 0; j < n; j++) {
                if(nums[i] == nums[j]) {
                    count++;
                }
            }

            if(count > n / 2) {
                return nums[i];
            }
        }

        return -1;
    }
};
```

### Complexity Analysis

- Time Complexity: **O(n²)**
- Space Complexity: **O(1)**

---

# Approach 2: Hash Map

Store the frequency of each element using a hash map.

## Algorithm

1. Traverse the array and store frequencies.
2. Traverse the map.
3. Return the element whose frequency is greater than `n/2`.

## Code

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int, int> mp;

        for(int num : nums) {
            mp[num]++;
        }

        for(auto it : mp) {
            if(it.second > nums.size() / 2) {
                return it.first;
            }
        }

        return -1;
    }
};
```

### Complexity Analysis

- Time Complexity: **O(n)**
- Space Complexity: **O(n)**

---

# Approach 3: Moore's Voting Algorithm (Optimal)

## Intuition

The majority element appears more than `n/2` times.

If we pair one occurrence of the majority element with one occurrence of any other element, the majority element will still remain.

Moore's Voting Algorithm uses this idea of **cancellation**.

---

## Algorithm

1. Initialize:
   - `count = 0`
   - `candidate = 0`

2. Traverse the array:
   - If `count == 0`, choose current element as candidate.
   - If current element equals candidate, increment count.
   - Otherwise, decrement count.

3. The final candidate will be the majority element.

---

## Optimal Code

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int count = 0;
        int candidate = 0;

        for(int num : nums) {
            if(count == 0) {
                candidate = num;
            }

            if(num == candidate) {
                count++;
            } else {
                count--;
            }
        }

        return candidate;
    }
};
```

### Complexity Analysis

- Time Complexity: **O(n)**
- Space Complexity: **O(1)**

---