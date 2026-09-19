# Subset Sum Equals to Target

---

# Problem Statement

Given an array of `N` positive integers and an integer `K`, determine whether there exists a **subset** whose sum is exactly equal to `K`.

Return

```text
true
```

if such a subset exists, otherwise return

```text
false
```

---

## Example 1

```text
Input

arr = [1,2,3,4]

k = 4
```

Output

```text
true
```

Explanation

```text
Subset

[4]

Sum = 4
```

---

# Pattern Recognition

Whenever you see

- Subset
- Pick / Not Pick
- Target Sum
- Can we form...
- Equal to K

Think

```text
DP on Subsequences
```

---

# Intuition

For every element, we have only **two choices**.

```text
Pick
```

OR

```text
Not Pick
```

We recursively try both possibilities.
If any one of them forms the target, the answer is

```text
true
```

---

# Recursive State

To uniquely identify every subproblem, we need

- Current Index
- Remaining Target

Hence,

```cpp
solve(index,target)
```

Meaning

> Can we form **target** using elements from index `0...index`?

---

# Recurrence

Two choices

## Not Pick

```cpp
solve(index-1,target)
```

---

## Pick

Only possible if

```cpp
arr[index] <= target
```

Then

```cpp
solve(index-1,target-arr[index])
```

---

Final Answer

```cpp
pick || notPick
```

---

# Approach 1 — Pure Recursion

## Base Cases

### Target becomes zero

We have successfully formed the required sum.

```cpp
if(target==0)
    return true;
```

---

### First element

If only one element is left, it must exactly equal the target.

```cpp
if(index==0)
    return arr[0]==target;
```

---

# Recursive Code

```cpp
class Solution {
public:

    bool solve(int index, int target, vector<int>& arr)
    {
        if(target == 0)
            return true;

        if(index == 0)
            return arr[0] == target;

        bool notPick = solve(index-1, target, arr);
        bool pick = false;

        if(arr[index] <= target)
        {
            pick = solve(index-1, target-arr[index], arr);
        }

        return pick || notPick;
    }

    bool subsetSumToK(int n, int k, vector<int>& arr)
    {
        return solve(n-1,k,arr);
    }
};
```

---

# Approach 2 — Memoization

## DP State

```cpp
dp[index][target]
```

Meaning

> Can we form **target** using elements `0...index`?

Store

```text
-1 → Not Computed

0 → False

1 → True
```

---

# Memoization Code

```cpp
class Solution {
public:

    bool solve(int index, int target, vector<int>& arr, vector<vector<int>>& dp)
    {
        if(target == 0)
            return true;

        if(index == 0)
            return arr[0] == target;

        if(dp[index][target] != -1)
            return dp[index][target];

        bool notPick = solve(index-1, target, arr, dp);
        bool pick = false;

        if(arr[index] <= target)
        {
            pick = solve(index-1, target-arr[index], arr, dp);
        }

        return dp[index][target] = pick || notPick;
    }

    bool subsetSumToK(int n, int k, vector<int>& arr)
    {
        vector<vector<int>> dp(n, vector<int>(k+1,-1));
        return solve(n-1,k,arr,dp);
    }
};
```

---

# Formula Used While Filling Every Cell

```cpp
notPick = dp[index-1][target];
```

---

```cpp
pick=false;

if(arr[index]<=target)

pick = dp[index-1]

[target-arr[index]];
```

---

```cpp
dp[index][target] = pick || notPick;
```

---

# Approach 3 — Tabulation

# Tabulation Code

```cpp
class Solution {
public:

    bool subsetSumToK(int n, int k, vector<int>& arr)
    {
        vector<vector<bool>> dp(n, vector<bool>(k+1,false));

        for(int i=0;i<n;i++)
            dp[i][0]=true;

        if(arr[0]<=k)
            dp[0][arr[0]]=true;

        for(int index=1;index<n;index++)
        {
            for(int target=1;
                target<=k;
                target++)
            {
                bool notPick=dp[index-1][target];
                bool pick=false;

                if(arr[index]<=target)
                {
                    pick=dp[index-1][target-arr[index]];
                }

                dp[index][target]=pick||notPick;
            }
        }
        return dp[n-1][k];
    }
};
```

---

# Line-by-Line Explanation

```cpp
dp[i][0]=true;
```

Target `0` is always possible by choosing an empty subset.

---

```cpp
dp[0][arr[0]]=true;
```

Using only the first element, we can form exactly its value.

---

```cpp
notPick=dp[index-1][target];
```

Ignore the current element.

---

```cpp
pick=dp[index-1][target-arr[index]];
```

Include the current element.

---

```cpp
dp[index][target] = pick||notPick;
```

If either choice works, the answer is true.

---

# Complexity Comparison

| Approach | Time | Space |
|----------|------|-------|
| Recursion | O(2ᴺ) | O(N) |
| Memoization | O(N×K) | O(N×K)+O(N) |
| Tabulation | O(N×K) | O(N×K) |
| Space Optimized | O(N×K) | O(K) |

---

# Pattern Recognition

Whenever you see

- Pick / Not Pick
- Target Sum
- Subset
- Equal to K
- Can we form...

Think

```text
DP on Subsequences
```

State

```cpp
(index,target)
```

---