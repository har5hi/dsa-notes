# LeetCode 992 - Subarrays with K Different Integers

---

# Problem Statement

Given an integer array `nums` and an integer `k`, return the **number of good subarrays**.

A **good subarray** is a contiguous subarray that contains **exactly `k` distinct integers**.

---

# Understanding the Problem

We need to count every subarray that contains

```text
Exactly K Different Numbers
```

Example

```
1 2 1 2 3
```

Valid

```
1 2

2 1

1 2 1

2 1 2

1 2 1 2
```

# Key Observation

Counting

```text
Exactly K
```

directly is difficult.

Instead,

count

```text
At Most K Distinct
```

and

```text
At Most (K-1) Distinct
```

Then subtract.

---

# The Formula

```text
Exactly(K) = AtMost(K) - AtMost(K-1)
```

This is the most important observation.

---

# Approaches

---

# 1. Brute Force

## Intuition

Generate every possible subarray.
For every subarray, count distinct elements using a HashSet.
If distinct count becomes exactly `k`, increase answer.
If it becomes greater than `k`, stop checking that starting index.

---

# Algorithm

1. Start from every index.
2. Keep inserting elements into a HashSet.
3. If distinct count equals `k`,
   increment answer.
4. If distinct count becomes greater than `k`,
   break.

---

# Brute Force Code

```cpp
class Solution {
public:

    int subarraysWithKDistinct(vector<int>& nums, int k) {

        int ans = 0;
        int n = nums.size();

        for(int i = 0; i < n; i++) {

            unordered_set<int> st;

            for(int j = i; j < n; j++) {

                st.insert(nums[j]);

                if(st.size() == k)
                    ans++;

                else if(st.size() > k)
                    break;
            }
        }
        return ans;
    }
};
```

---

# Complexity

Time

```text
O(n²)
```

Space

```text
O(k)
```

---

# 2. Optimal Approach (Sliding Window)

---

# Intuition

Instead of counting

```text
Exactly K
```

directly,

count

```text
At Most K
```

using Sliding Window.

Then use

```cpp
Exactly(K) = AtMost(K) - AtMost(K-1)
```

---

# Algorithm

1. Expand the window.
2. Store frequencies in a HashMap.
3. If distinct elements become greater than `K`,
   shrink the window.
4. Add

```cpp
right-left+1
```

to the answer.
5. Return answer.

---

# Optimal Code (Fully Commented)

```cpp
class Solution {
public:

    // Counts subarrays having at most k distinct integers
    int atMost(vector<int>& nums, int k) {

        if(k < 0)
            return 0;

        unordered_map<int,int> freq;

        int left = 0;
        int ans = 0;

        for(int right = 0; right < nums.size(); right++) {

            // Include current element
            freq[nums[right]]++;

            // Shrink window until distinct <= k
            while(freq.size() > k) {

                freq[nums[left]]--;

                // Remove element completely if frequency becomes zero
                if(freq[nums[left]] == 0)
                    freq.erase(nums[left]);
                left++;
            }

            // Count all valid subarrays ending at right
            ans += (right-left+1);
        }
        return ans;
    }

    int subarraysWithKDistinct(vector<int>& nums, int k) {
        return atMost(nums,k) - atMost(nums,k-1);
    }
};
```

---

# Complexity

### Time

```text
O(n)
```

Each element enters and leaves the window only once.

---

### Space

```text
O(k)
```

HashMap stores at most `k+1` distinct elements.

---

# Why Sliding Window Works

The window always satisfies

```text
Distinct ≤ K
```

Whenever it becomes invalid,

we move `left` until it becomes valid again.

For every valid window,

there are

```cpp
right-left+1
```

subarrays ending at `right`.

Thus,

the answer is computed in linear time.

---