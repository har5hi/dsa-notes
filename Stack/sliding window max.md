# LeetCode 239 - Sliding Window Maximum

---

# Problem Statement

Given an integer array `nums` and an integer `k`, there is a sliding window of size `k` moving from the left to the right.

Return the **maximum element** in every window.

---

## Example 1

```text
Input

nums = [1,3,-1,-3,5,3,6,7]

k = 3
```

Output

```text
[3,3,5,5,6,7]
```

---

# Brute Force Approach

For every window, scan all `k` elements.
Find the maximum.

---

## Code

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {

        int n = nums.size();
        vector<int> ans;

        for(int i=0;i<=n-k;i++){
            int mx = nums[i];

            for(int j=i;j<i+k;j++)
                mx = max(mx, nums[j]);

            ans.push_back(mx);
        }
        return ans;
    }
};
```

---

### Time Complexity

```
O(n × k)
```

---

### Space Complexity

```
O(1)
```

---

# Optimal Intuition

Instead of checking all `k` elements every time, keep only the **useful candidates** for maximum.
Use a **Monotonic Decreasing Deque**.

---

## Why Deque?

We need to

- remove elements from the front (window moves)
- remove smaller elements from the back
- access the maximum in O(1)

A deque supports all of these efficiently.

---

# Monotonic Queue Property

The deque always stores **indices** in **decreasing order of values**.

Example

```
nums

1 3 -1
```

Deque stores

```
3

-1
```

Not

```
1

3

-1
```

because

```
1
```

can never become the maximum once

```
3
```

is inside the window.

---

# Optimal Code

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {

        deque<int> dq;
        vector<int> ans;

        for(int i=0;i<nums.size();i++){

            // Remove indices outside window
            while(!dq.empty() && dq.front() <= i-k)
                dq.pop_front();

            // Remove smaller elements
            while(!dq.empty() && nums[dq.back()] < nums[i])
                dq.pop_back();

            dq.push_back(i);

            // Window formed
            if(i >= k-1)
                ans.push_back(nums[dq.front()]);
        }
        return ans;
    }
};
```

---

# Why Store Indices Instead of Values?

Suppose

```
nums

3 1 2
```

Window moves.

Need to know whether

```
3
```

is still inside the window.

Only indices can tell this.

---

# Time Complexity

Each element is

- inserted once
- removed once

```
O(n)
```

---

# Space Complexity

Deque stores at most

```
k
```

elements.

```
O(k)
```

---

# Interview Tips

Whenever the problem asks

- Sliding Window Maximum
- Sliding Window Minimum
- First Negative in Window

Think

> **Deque (Monotonic Queue)**

Not Stack.

---