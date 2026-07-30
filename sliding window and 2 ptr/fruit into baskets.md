# Fruit Into Baskets (LeetCode 904)

---

# Problem Statement

There is only one row of fruit trees on the farm.
An integer array called `fruits` represents the trees, where

```text
fruits[i]
```

denotes the type of fruit produced by the **ith tree**.

You have **two baskets**.

Rules:

- Each basket can contain **only one type of fruit**.
- Each basket can hold **unlimited quantity** of that fruit.
- You may start from **any tree**.
- Move only towards the **right**.
- Pick exactly **one fruit from every tree**.
- If you encounter a fruit that cannot fit into either basket, you must stop.

Return the **maximum number of fruits** that can be collected.

---

# Key Observation

Each basket stores only

```text
ONE
```

fruit type.

Therefore,

our window can contain

```text
At Most 2 Distinct Fruits
```

This problem is exactly

> **Longest Subarray with At Most 2 Distinct Elements**

---

# Approaches

---

# 1. Brute Force

## Intuition

Generate every possible subarray.

For each subarray, count the number of distinct fruit types.

If distinct fruits are

```text
≤ 2
```

update the answer.

Otherwise, stop extending that subarray.

---

# Algorithm

1. Choose every starting index.
2. Extend the subarray.
3. Store fruit types in a Hash Set.
4. If distinct fruits exceed 2,
   stop.
5. Otherwise,
   update maximum length.

---

# Brute Force Code

```cpp
class Solution {
public:

    int totalFruit(vector<int>& fruits) {

        int n = fruits.size();
        int maxLen = 0;

        for(int i = 0; i < n; i++) {
            unordered_set<int> st;

            for(int j = i; j < n; j++) {

                st.insert(fruits[j]);

                if(st.size() > 2)
                    break;

                maxLen = max(maxLen, j - i + 1);
            }
        }
        return maxLen;
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
O(2)

≈ O(1)
```

---

# 2. Optimal Approach (Sliding Window)

---

# Intuition

Maintain one window.

Expand it using the

```text
right
```

pointer.

Store frequencies of fruit types inside a HashMap.

As long as

```text
Distinct Fruits ≤ 2
```

the window is valid.

If distinct fruits become

```text
3
```

move the

```text
left
```

pointer until only

```text
2
```

fruit types remain.

Update the maximum window length.

---

# Algorithm

1. Create a HashMap.
2. Expand using the right pointer.
3. Store fruit frequencies.
4. If distinct fruits become more than 2,
   shrink the window.
5. Update answer.
6. Return maximum length.

---

# Optimal Code (Fully Commented)

```cpp
class Solution {
public:

    int totalFruit(vector<int>& fruits) {

        // Stores frequency of each fruit type
        unordered_map<int,int> freq;

        // Left boundary of the window
        int left = 0;

        // Maximum valid window length
        int maxLen = 0;

        // Expand the window
        for(int right = 0; right < fruits.size(); right++) {

            // Add current fruit
            freq[fruits[right]]++;

            // More than 2 fruit types
            while(freq.size() > 2) {

                // Remove left fruit
                freq[fruits[left]]--;

                // Remove fruit type completely
                if(freq[fruits[left]] == 0)
                    freq.erase(fruits[left]);
                left++;
            }
            // Current window is valid
            maxLen = max(maxLen, right - left + 1);
        }
        return maxLen;
    }
};
```

---

# Complexity

Time

```text
O(n)
```

Space

```text
O(2)

≈ O(1)
```

---