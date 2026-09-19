# LeetCode 416 - Partition Equal Subset Sum

---

# Problem Statement

Given an integer array `nums`, determine whether it can be divided into **two subsets** such that the **sum of both subsets is equal**.

Return

```text
true
```

if such a partition exists, otherwise return

```text
false
```

---

## Example 1

```text
Input

nums = [1,5,11,5]
```

Output

```text
true
```

Explanation

```text
Subset 1

1 + 5 + 5 = 11

Subset 2

11

Both sums are equal.
```

---

# Observation (Most Important)

Suppose

```text
Total Sum = S
```

If we divide the array into two equal subsets,

```text
Subset1 + Subset2 = S
```

Since

```text
Subset1 = Subset2
```

Both must be

```text
S / 2
```

Therefore, instead of finding **two subsets**,

our problem becomes

> **Can we find one subset whose sum is `S/2`?**

This converts the problem into **Subset Sum Equals to Target**.

---

# Reduction to Subset Sum

Example

```text
nums = [1,5,11,5]
```

Total Sum

```text
22
```

Required Target

```text
22 / 2

=

11
```

Now the question becomes

```text
Can we form

11

using some subset?
```

Yes. Therefore, answer is

```text
true
```

---

# Important Observation

If

```text
Total Sum
```

is odd, equal partition is impossible.

Example

```text
Sum = 13
```

Cannot divide

13

into

```text
6.5

6.5
```

Therefore,

```cpp
if(sum % 2 != 0)
return false;
```

---

# DP State

Exactly same as Subset Sum.

```cpp
dp[index][target]
```

Meaning

> Can we form **target** using elements from `0...index`?

---

# Approach 1 — Pure Recursion

# Recursive Code

```cpp
class Solution {
public:

    bool solve(int index, int target, vector<int>& nums)
    {
        if(target==0)
            return true;

        if(index==0)
            return nums[0]==target;

        bool notPick= solve(index-1, target, nums);
        bool pick=false;

        if(nums[index]<=target)
        {
            pick= solve(index-1, target-nums[index], nums);
        }
        return pick||notPick;
    }

    bool canPartition(vector<int>& nums)
    {
        int sum=0;

        for(int x:nums)
            sum+=x;

        if(sum%2)
            return false;

        return solve(nums.size()-1, sum/2, nums);
    }
};
```

---

# Approach 2 — Memoization

# Memoization Code

```cpp
class Solution {
public:

    bool solve(int index, int target, vector<int>& nums, vector<vector<int>>& dp)
    {
        if(target==0)
            return true;

        if(index==0)
            return nums[0]==target;

        if(dp[index][target]!=-1)
            return dp[index][target];

        bool notPick= solve(index-1,target, nums,dp);

        bool pick=false;

        if(nums[index]<=target)
        {
            pick=solve(index-1,target-nums[index],nums,dp);
        }

        return dp[index][target]=pick||notPick;
    }

    bool canPartition(vector<int>& nums)
    {
        int sum=0;

        for(int x:nums)
            sum+=x;

        if(sum%2)
            return false;

        int target=sum/2;

        vector<vector<int>> dp(nums.size(),vector<int>(target+1,-1));

        return solve(nums.size()-1,target,nums,dp);
    }
};
```

---

# Formula Used While Filling Every Cell

```cpp
bool notPick = dp[index-1][target];
```

---

```cpp
bool pick = false;

if(nums[index] <= target)

pick=dp[index-1][target-nums[index]];
```

---

```cpp
dp[index][target] = pick || notPick;
```

---

# Approach 3 — Tabulation

```cpp
class Solution {
public:

    bool canPartition(vector<int>& nums)
    {
        int sum=0;

        for(int x:nums)
            sum+=x;

        if(sum%2)
            return false;

        int target=sum/2;
        int n=nums.size();

        vector<vector<bool>> dp(n,vector<bool>(target+1,false));

        for(int i=0;i<n;i++)
            dp[i][0]=true;

        if(nums[0]<=target)
            dp[0][nums[0]]=true;

        for(int index=1;index<n;index++)
        {
            for(int t=1;t<=target;t++)
            {
                bool notPick=dp[index-1][t];

                bool pick=false;

                if(nums[index]<=t)
                {
                    pick=dp[index-1][t-nums[index]];
                }

                dp[index][t]=pick||notPick;
            }
        }
        return dp[n-1][target];
    }
};
```

---

# Line-by-Line Explanation

```cpp
sum += nums[i];
```

Calculate the total sum of the array.

---

```cpp
if(sum % 2)
    return false;
```

An odd total sum can never be divided into two equal halves.

---

```cpp
target = sum / 2;
```

Now the problem becomes:

```text
Can we form sum/2 using a subset?
```

---

```cpp
pick || notPick
```

If either choice succeeds, the partition is possible.

---

# Complexity Comparison

| Approach | Time | Space |
|----------|------|-------|
| Recursion | O(2ᴺ) | O(N) |
| Memoization | O(N×Target) | O(N×Target)+O(N) |
| Tabulation | O(N×Target) | O(N×Target) |
| Space Optimized | O(N×Target) | O(Target) |

---