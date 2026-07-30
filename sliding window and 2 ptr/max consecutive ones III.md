# LeetCode 1004 - Max Consecutive Ones III

---

# Problem Statement

Given a binary array `nums` and an integer `k`, return the **maximum number of consecutive 1's** in the array if you can flip at most `k` zeros.

You may flip **at most `k` zeros into 1s**.

---

# Key Observation

Instead of counting

```text
Number of Ones
```

count

```text
Number of Zeros
```

Why?

Because

zeros are the only problem.

If the number of zeros inside the window is

```text
≤ k
```

then we can flip all of them.

The window becomes completely filled with ones.

---

# Approaches

---

# 1. Brute Force

## Intuition

Generate every possible subarray.

For every subarray, count the number of zeros.

If

```text
zeros ≤ k
```

update the answer.

---

# Algorithm

1. Generate every subarray.
2. Count zeros.
3. If zeros ≤ k,
   update maximum length.
4. Return answer.

---

# Brute Force Code

```cpp
class Solution {
public:

    int longestOnes(vector<int>& nums, int k) {

        int n = nums.size();
        int maxLen = 0;

        for(int i = 0; i < n; i++) {
            int zeroCount = 0;

            for(int j = i; j < n; j++) {

                if(nums[j] == 0)
                    zeroCount++;

                if(zeroCount <= k)
                    maxLen = max(maxLen, j - i + 1);
                else
                    break;
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
O(1)
```

---

# 2. Optimal Approach (Sliding Window)

---

# Intuition

Maintain one window.

Expand it using the right pointer.

Keep counting zeros.

As long as

```text
zeroCount ≤ k
```

the window is valid.

If

```text
zeroCount > k
```

shrink the window from the left until it becomes valid again.

Keep updating the maximum window size.

---

# Algorithm

1. Initialize two pointers (`left` and `right`).
2. Expand using `right`.
3. Count zeros.
4. If zeros exceed `k`,
   move `left` until zeros become ≤ `k`.
5. Update maximum length.
6. Return answer.

---

# Optimal Code (Fully Commented)

```cpp
class Solution {
public:

    int longestOnes(vector<int>& nums, int k) {

        // Left boundary of the window
        int left = 0;

        // Number of zeros inside the window
        int zeroCount = 0;

        // Stores maximum valid window length
        int maxLen = 0;

        // Expand window
        for(int right = 0; right < nums.size(); right++) {

            // If current element is zero,
            // include it in the count.
            if(nums[right] == 0)
                zeroCount++;

            // Window becomes invalid.
            while(zeroCount > k) {

                // Remove left element.
                if(nums[left] == 0)
                    zeroCount--;

                left++;
            }

            // Current window is valid.
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
O(1)
```

---

# Why is the Time Complexity O(n)?

Although there is

```cpp
for

+

while
```

the complexity is **not O(n²)**.

Why?

Because

- `right` moves from left to right only once.
- `left` also moves from left to right only once.

Neither pointer ever moves backwards.

Total pointer movements

```text
≈ 2n
```

which simplifies to

```text
O(n)
```

---