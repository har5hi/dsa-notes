# LeetCode 62 - Unique Paths

---

# Problem Statement

A robot is initially located at the **top-left corner** of an `m x n` grid.
The robot wants to reach the **bottom-right corner**.
The robot can only move:

- Right
- Down

Return the **number of unique paths** from the start to the destination.

---

## Example 1

```text
Input:

m = 3
n = 7

Output:

28
```

---

# Intuition

Suppose we want to reach

```text
(i,j)
```

How can we reach this cell?
There are only **two possible ways**.

```
(i-1,j)

↓

Down
```

OR

```
(i,j-1)

→ Right
```

No other move is allowed.

Therefore,

```
Unique Paths(i,j) = Paths from Top + Paths from Left
```

This becomes our DP transition.

---

# DP State

Define

```cpp
dp[i][j]
```

Meaning

> Number of unique paths from `(0,0)` to `(i,j)`.

---

# Recurrence Relation

If we are at

```
(i,j)
```

then

```text
dp[i][j] = dp[i-1][j] + dp[i][j-1]
```

because
- Top contributes all paths reaching this cell.
- Left contributes all paths reaching this cell.

---

# Approach 1 — Pure Recursion

## Idea

To reach

```
(i,j)
```

we can come from

```
Top

OR

Left
```

Solve both recursively.

---

# Base Cases

Destination reached

```cpp
if(i==0 && j==0)

return 1;
```

---

Outside Grid

```cpp
if(i<0 || j<0)

return 0;
```

---

# Recursive Code

```cpp
class Solution {
public:

    int solve(int i, int j)
    {
        if(i == 0 && j == 0)
            return 1;

        if(i < 0 || j < 0)
            return 0;

        int up = solve(i - 1, j);
        int left = solve(i, j - 1);
        return up + left;
    }

    int uniquePaths(int m, int n)
    {
        return solve(m - 1, n - 1);
    }
};
```

---

# Time Complexity

```
O(2^(m+n))
```

---

# Space Complexity

```
O(m+n)
```

Recursive stack.

---

# Approach 2 — Memoization

## DP State

```cpp
dp[i][j]
```

stores

```
Unique paths to

(i,j)
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

    int solve(int i, int j, vector<vector<int>>& dp)
    {
        if(i == 0 && j == 0)
            return 1;

        if(i < 0 || j < 0)
            return 0;

        if(dp[i][j] != -1)
            return dp[i][j];

        int up = solve(i - 1, j, dp);
        int left = solve(i, j - 1, dp);
        return dp[i][j] = up + left;
    }

    int uniquePaths(int m, int n)
    {
        vector<vector<int>> dp(m,vector<int>(n, -1));
        return solve(m - 1,n - 1,dp);
    }
};
```

---

# Approach 3 — Tabulation

## Algorithm

```
dp[0][0]=1

for every cell

up = dp[i-1][j]

left = dp[i][j-1]

dp[i][j] = up+left
```

---

# Tabulation Code

```cpp
class Solution {
public:

    int uniquePaths(int m, int n)
    {
        vector<vector<int>> dp(m, vector<int>(n, 0));

        dp[0][0] = 1;

        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                if(i == 0 && j == 0)
                    continue;

                int up = 0;
                int left = 0;

                if(i > 0)
                    up = dp[i-1][j];

                if(j > 0)
                    left = dp[i][j-1];

                dp[i][j] = up + left;
            }
        }
        return dp[m-1][n-1];
    }
};
```

---

# Time Complexity

```
O(m × n)
```

---

# Space Complexity

```
O(m × n)
```

---

# Complexity Comparison

| Approach | Time | Space |
|----------|------|-------|
| Recursion | O(2^(m+n)) | O(m+n) |
| Memoization | O(m×n) | O(m×n)+O(m+n) |
| Tabulation | O(m×n) | O(m×n) |
| Space Optimized | O(m×n) | O(n) |

---

# Pattern Recognition

Whenever you see

- Grid
- Count number of ways
- Right / Down movement
- Previous row and previous column

Think

```
DP on Grids
```

---