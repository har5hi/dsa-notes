# LeetCode 930 - Binary Subarrays With Sum

---

# Problem Statement

Given a binary array `nums` and an integer `goal`, return the number of **non-empty subarrays** with a sum equal to `goal`.

A **subarray** is a contiguous part of the array.

---

# Example 1

```text
Input

nums = [1,0,1,0,1]

goal = 2
```

Output

```text
4
```

Explanation

The valid subarrays are

```text
[1,0,1]

[1,0,1,0]

[0,1,0,1]

[1,0,1]
```

Answer

```text
4
```

---

# Key Observation

Finding

```text
Exactly Goal
```

directly is difficult.

Instead,

find

```text
At Most Goal
```

and

```text
At Most (Goal-1)
```

Then

```text
Exactly Goal = At Most Goal - At Most (Goal-1)
```

This trick is used in many sliding window problems.

---

# Why Does This Work?

Suppose

```text
At Most 2 = 15
```

subarrays.

Among them,

```text
At Most 1 = 11
```

subarrays.

Then remaining

```text
15-11 = 4
```

must have

```text
Exactly 2
```

---

# Approaches

---

# 1. Brute Force

## Intuition

---

# Algorithm

1. Generate every subarray.
2. Calculate its sum.
3. If sum equals goal,
   increment answer.
4. Return answer.

---

# Brute Force Code

```cpp
class Solution {
public:

    int numSubarraysWithSum(vector<int>& nums, int goal) {

        int n = nums.size();
        int ans = 0;

        for(int i = 0; i < n; i++) {
            int sum = 0;

            for(int j = i; j < n; j++) {
                sum += nums[j];

                if(sum == goal)
                    ans++;
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
O(1)
```

---

# 2. Optimal Approach (Sliding Window)

---

# Intuition

Instead of counting

```text
Exactly Goal
```

directly,

count

```text
At Most Goal
```

Then

count

```text         
At Most Goal-1
```

Subtract both.

---

# Sliding Window Idea

Maintain a window.

Expand using

```text
right
```

Keep track of

```text
Window Sum
```

If

```text
Sum > Goal
```

move

```text
left
```

until

```text
Sum ≤ Goal
```

Now,

every subarray ending at

```text
right
```

and starting anywhere between

```text
left
```

and

```text
right
```

is valid.

Number of such subarrays

```text
right-left+1
```

---

# Algorithm

For

```text
atMost(goal)
```

1. Expand the window.
2. Add current element.
3. While sum exceeds goal,
   shrink the window.
4. Every valid window contributes

```text
right-left+1
```

subarrays.

Finally

```text
Answer = atMost(goal) - atMost(goal-1)
```

---

# Optimal Code (Fully Commented)

```cpp
class Solution {
public:

    // Counts subarrays having sum <= goal
    int atMost(vector<int>& nums, int goal) {

        // Impossible case
        if(goal < 0)
            return 0;

        int left = 0;
        int sum = 0;
        int ans = 0;

        for(int right = 0; right < nums.size(); right++) {

            // Expand window
            sum += nums[right];

            // Shrink until valid
            while(sum > goal) {
                sum -= nums[left];
                left++;
            }
            // Count all valid subarrays
            ans += (right-left+1);
        }
        return ans;
    }

    int numSubarraysWithSum(vector<int>& nums, int goal) {
        return atMost(nums,goal) - atMost(nums,goal-1);
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

# LeetCode 1248 - Count Number of Nice Subarrays (Optimal)

## Key Observation

A **nice subarray** is a subarray containing **exactly `k` odd numbers**.

Instead of counting **exactly `k` odd numbers** directly,

count

```text
At Most k Odd Numbers
```

and

```text
At Most (k-1) Odd Numbers
```

Then

```text
Exactly k

=

At Most(k)

-

At Most(k-1)
```

This is the same pattern used in **LC 930 - Binary Subarrays With Sum**.

---

# Intuition

Maintain a sliding window.

- Expand the window using the `right` pointer.
- Count the number of odd elements inside the window.
- If odd numbers become greater than `k`, shrink the window.
- Every valid window contributes

```text
right - left + 1
```

valid subarrays.

Finally,

```cpp
Exactly(k) = atMost(k) - atMost(k-1)
```

---

# Algorithm

### Function: `atMost(k)`

1. Initialize `left = 0`, `oddCount = 0`, `ans = 0`.
2. Expand the window using `right`.
3. If the current number is odd, increment `oddCount`.
4. While `oddCount > k`, shrink the window from the left.
5. Add `(right - left + 1)` to the answer.
6. Return the count.

Finally,

```cpp
return atMost(nums, k) - atMost(nums, k - 1);
```

---

# Code (Fully Commented)

```cpp
class Solution {
public:

    // Counts subarrays having at most 'goal' odd numbers
    int atMost(vector<int>& nums, int goal) {

        // Impossible case
        if(goal < 0)
            return 0;

        int left = 0;
        int oddCount = 0;
        int ans = 0;

        for(int right = 0; right < nums.size(); right++){
            
            // Add current number if it is odd
            oddCount += nums[right] % 2;

            // Shrink window until it has at most 'goal' odd numbers
            while(oddCount > goal) {

                // Remove left element if it is odd
                oddCount -= nums[left] % 2;

                left++;
            }

            // Count all valid subarrays ending at 'right'
            ans += (right - left + 1);
        }

        return ans;
    }

    int numberOfSubarrays(vector<int>& nums, int k) {

        // Exactly K = AtMost(K) - AtMost(K-1)
        return atMost(nums, k) - atMost(nums, k - 1);
    }
};
```

---

# Complexity

**Time:** `O(n)`

**Space:** `O(1)`

---

# Pattern Comparison

| LC 930 | LC 1248 |
|---------|----------|
| Count **sum** | Count **odd numbers** |
| `sum += nums[right]` | `oddCount += nums[right] % 2` |
| `while(sum > goal)` | `while(oddCount > goal)` |
| `sum -= nums[left]` | `oddCount -= nums[left] % 2` |
| `Exactly(goal) = AtMost(goal) - AtMost(goal-1)` | `Exactly(k) = AtMost(k) - AtMost(k-1)` |

---