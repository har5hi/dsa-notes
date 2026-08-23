# LeetCode 2104 - Sum of Subarray Ranges

---

# Problem Statement

Given an integer array `nums`, return the **sum of the range of every subarray**.

The **range** of a subarray is

```text
Maximum Element − Minimum Element
```

---

## Example 1

```text
Input

nums = [1,2,3]
```

Output

```text
4
```

### Explanation

All subarrays

```text
[1]      Range = 0

[1,2]    Range = 1

[1,2,3]  Range = 2

[2]      Range = 0

[2,3]    Range = 1

[3]      Range = 0
```

Total

```text
0+1+2+0+1+0 = 4
```

---

# Brute Force Approach

Generate every subarray.

Maintain

- Maximum
- Minimum

while expanding the subarray.

Add

```text
Maximum - Minimum
```

---

## Code

```cpp
class Solution {
public:
    long long subArrayRanges(vector<int>& nums) {

        int n = nums.size();
        long long ans = 0;

        for(int i=0;i<n;i++){
            int mini = nums[i];
            int maxi = nums[i];
            for(int j=i;j<n;j++){
                mini = min(mini, nums[j]);
                maxi = max(maxi, nums[j]);
                ans += (maxi - mini);
            }
        }
        return ans;
    }
};
```

---

### Time Complexity

```
O(n²)
```

### Space Complexity

```
O(1)
```

---

# Optimal Intuition

Instead of computing the range of every subarray, think in terms of **contribution**.

Every element contributes

- positively when it is the **maximum**
- negatively when it is the **minimum**

So

```text
Answer = Sum(Maximum Contribution) - Sum(Minimum Contribution)
```

This converts the problem into

- Sum of Subarray Maximums
- Sum of Subarray Minimums (LC 907)

---

# Key Observation

For every element

```
Contribution as Maximum = nums[i] × (Number of subarrays where it is maximum)
```

Similarly

```
Contribution as Minimum = nums[i] × (Number of subarrays where it is minimum)
```

---

# Counting Contribution

For every element

Find

- Previous Greater
- Next Greater

to calculate maximum contribution.

Find

- Previous Smaller
- Next Smaller

to calculate minimum contribution.

---

# Distance Formula

Suppose

```
left = distance to previous boundary
```

```
right = distance to next boundary
```

Then

```
Number of subarrays = left × right
```

Contribution

```
nums[i] × left × right
```

---

# Duplicate Handling

## For Maximum

Previous Greater

```cpp
<
```

Next Greater

```cpp
<=
```

---

## For Minimum

Previous Smaller

```cpp
>
```

Next Smaller

```cpp
>=
```

Exactly one side is strict.

This prevents duplicate counting.

---

# Optimal Code

```cpp
class Solution {
public:

    long long subArrayRanges(vector<int>& nums) {

        int n = nums.size();

        vector<int> prevGreater(n), nextGreater(n);
        vector<int> prevSmaller(n), nextSmaller(n);

        stack<int> st;

        // Previous Greater

        for(int i=0;i<n;i++){
            while(!st.empty() && nums[st.top()] < nums[i])
                st.pop();
            prevGreater[i] = st.empty() ? -1 : st.top();
            st.push(i);
        }

        while(!st.empty()) st.pop();

        // Next Greater

        for(int i=n-1;i>=0;i--){
            while(!st.empty() && nums[st.top()] <= nums[i])
                st.pop();
            nextGreater[i] = st.empty() ? n : st.top();
            st.push(i);
        }

        while(!st.empty()) st.pop();

        // Previous Smaller

        for(int i=0;i<n;i++){
            while(!st.empty() && nums[st.top()] > nums[i])
                st.pop();
            prevSmaller[i] = st.empty() ? -1 : st.top();
            st.push(i);
        }
        while(!st.empty()) st.pop();

        // Next Smaller

        for(int i=n-1;i>=0;i--){
            while(!st.empty() && nums[st.top()] >= nums[i])
                st.pop();
            nextSmaller[i] = st.empty() ? n : st.top();
            st.push(i);
        }

        long long maxSum = 0;
        long long minSum = 0;

        for(int i=0;i<n;i++){
            long long left = i - prevGreater[i];
            long long right = nextGreater[i] - i;
            maxSum += 1LL * nums[i] * left * right;
        }

        for(int i=0;i<n;i++){
            long long left = i - prevSmaller[i];
            long long right = nextSmaller[i] - i;
            minSum += 1LL * nums[i] * left * right;
        }

        return maxSum - minSum;
    }
};
```

---

### Contribution

```cpp
left = i - previous

right = next - i

Contribution = nums[i] × left × right
```

---

# Why Different Comparisons?

This is the most frequently asked interview question.

For duplicate elements,

exactly one occurrence should own every subarray.

Hence

### Maximum

| Boundary | Comparison |
|----------|------------|
| Previous Greater | `<` |
| Next Greater | `<=` |

---

### Minimum

| Boundary | Comparison |
|----------|------------|
| Previous Smaller | `>` |
| Next Smaller | `>=` |

One side is **strict**, the other is **non-strict**.

---

# Time Complexity

Each element is

- pushed once
- popped once

Overall

```
O(n)
```

---

# Space Complexity

Arrays

```
O(n)
```

Stack

```
O(n)
```

Total

```
O(n)
```

---

# Interview Tips

Whenever the problem asks

- Sum of Maximums
- Sum of Minimums
- Sum of Ranges

Think

> **Contribution Technique + Monotonic Stack**

Break the problem into

```
Maximum Contribution − Minimum Contribution
```

instead of processing every subarray.

---