# LeetCode 120 - Triangle

---

# Problem Statement

Given a triangle array, return the **minimum path sum** from top to bottom.

At every step, you may move to:

- Same column
- Next column

If you are at

```text
(i,j)
```

you can move to

```text
(i+1,j)
```

or

```text
(i+1,j+1)
```

Return the minimum possible path sum.

---

## Example

```text
Input

[
     [2],
    [3,4],
   [6,5,7],
 [4,1,8,3]
]
```

Output

```text
11
```

---

# Intuition

Suppose we are standing at

```text
(i,j)
```

There are only two choices.

Move

```text
↓

(i+1,j)
```

OR

Move

```text
↘

(i+1,j+1)
```

Choose the path with minimum sum.

---

# DP State

```cpp
dp[i][j]
```

Meaning

> Minimum path sum from cell `(i,j)` to the last row.

Notice

Unlike previous grid problems, here DP stores

```text
Current Cell
↓
Destination
```

instead of

```text
Source
↓
Current Cell
```

---

# Recurrence

```cpp
dp[i][j] = triangle[i][j] + min(dp[i+1][j], dp[i+1][j+1])
```

---

# Approach 1 — Pure Recursion

## Base Case

If we reach the last row, there is nowhere else to go.
Return the current value.

```cpp
if(i==n-1)
return triangle[i][j];
```

---

# Recursive Code

```cpp
class Solution {
public:

    int solve(int i, int j, vector<vector<int>>& triangle, int n)
    {
        if(i == n-1)
            return triangle[i][j];

        int down = triangle[i][j] + solve(i+1,j,triangle,n);
        int diagonal = triangle[i][j] + solve(i+1,j+1,triangle,n);
        return min(down,diagonal);
    }

    int minimumTotal(vector<vector<int>>& triangle)
    {
        int n = triangle.size();
        return solve(0,0,triangle,n);
    }
};
```

---

# Approach 2 — Memoization

## DP State

```cpp
dp[i][j]
```

stores

```text
Minimum path sum

from

(i,j)

↓

last row
```

Initially

```text
All = -1
```

---

# Memoization Code

```cpp
class Solution {
public:

    int solve(int i, int j, vector<vector<int>>& triangle, vector<vector<int>>& dp,int n)
    {
        if(i == n-1)
            return triangle[i][j];

        if(dp[i][j] != -1)
            return dp[i][j];

        int down = triangle[i][j] + solve(i+1,j,triangle,dp,n);
        int diagonal = triangle[i][j] + solve(i+1,j+1,triangle,dp,n);
        return dp[i][j] = min(down,diagonal);
    }

    int minimumTotal(vector<vector<int>>& triangle)
    {
        int n = triangle.size();

        vector<vector<int>> dp(n, vector<int>(n,-1));

        return solve(0,0,triangle,dp,n);
    }
};
```

---

# Formula Used While Filling Every Cell

```cpp
dp[i][j] = triangle[i][j] + min( dp[i+1][j], dp[i+1][j+1])
```

Notice

We fill

```text
Bottom

↑

Top
```

because every cell depends on the row below.

---

# Approach 3 — Tabulation

---

# Tabulation Code

```cpp
class Solution {
public:

    int minimumTotal(vector<vector<int>>& triangle)
    {
        int n = triangle.size();

        vector<vector<int>> dp = triangle;

        for(int i=n-2;i>=0;i--)
        {
            for(int j=0;j<=i;j++)
            {
                dp[i][j] = triangle[i][j] + min(dp[i+1][j], dp[i+1][j+1]);
            }
        }
        return dp[0][0];
    }
};
```

---

# Space Optimization

Observation

Each row depends only on the row below.

So instead of

```text
O(n²)
```

store only one row.

---

# Space Optimized Code

```cpp
class Solution {
public:

    int minimumTotal(vector<vector<int>>& triangle)
    {
        int n = triangle.size();

        vector<int> front =
            triangle[n-1];

        for(int i=n-2;i>=0;i--)
        {
            vector<int> curr(n,0);

            for(int j=0;j<=i;j++)
            {
                curr[j] =
                    triangle[i][j]
                    +
                    min(
                        front[j],
                        front[j+1]
                    );
            }

            front = curr;
        }

        return front[0];
    }
};
```

---

# Complexity Comparison

| Approach | Time | Space |
|----------|------|-------|
| Recursion | O(2ⁿ) | O(n) |
| Memoization | O(n²) | O(n²)+O(n) |
| Tabulation | O(n²) | O(n²) |
| Space Optimized | O(n²) | O(n) |

---