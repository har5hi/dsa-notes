# LeetCode 64 - Minimum Path Sum

---

# Problem Statement

Given an `m x n` grid filled with **non-negative numbers**, find a path from the **top-left** corner to the **bottom-right** corner such that the **sum of all numbers along the path is minimum**.

You can move only:

- Right
- Down

Return the **minimum path sum**.

---

## Example 1

```text
Input

1 3 1

1 5 1

4 2 1
```

Output

```text
7
```

Path

```text
1 → 3 → 1 → 1 → 1

Sum = 7
```

---

## Example 2

```text
Input

1 2 3

4 5 6
```

Output

```text
12
```

---

# Intuition

Suppose we want the minimum cost to reach

```text
(i,j)
```

The last move must have come from

```
Top

↓

(i-1,j)
```

OR

```
Left → (i,j-1)
```

Choose whichever gives the smaller total cost.

---

# Recurrence

```text
dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])
```

---

# Approach 1 — Pure Recursion

## Base Cases

Reached source

```cpp
if(i==0 && j==0)
return grid[0][0];
```

---

Outside grid
Return a very large value because this path is invalid.

```cpp
return 1e9;
```

Using `1e9` ensures this path is never chosen in the `min()` operation.

---

# Recursive Code

```cpp
class Solution {
public:

    int solve(int i, int j, vector<vector<int>>& grid)
    {
        if(i == 0 && j == 0)
            return grid[0][0];

        if(i < 0 || j < 0)
            return 1e9;

        int up = grid[i][j] + solve(i-1,j,grid);

        int left = grid[i][j] + solve(i,j-1,grid);

        return min(up,left);
    }

    int minPathSum(vector<vector<int>>& grid)
    {
        int m = grid.size();
        int n = grid[0].size();

        return solve(m-1,n-1,grid);
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

```
Minimum cost
to reach

(i,j)
```

Initially

```
All = -1
```

---

# Memoization Code

```cpp
class Solution {
public:

    int solve(int i, int j, vector<vector<int>>& grid, vector<vector<int>>& dp)
    {
        if(i == 0 && j == 0)
            return grid[0][0];

        if(i < 0 || j < 0)
            return 1e9;

        if(dp[i][j] != -1)
            return dp[i][j];

        int up = grid[i][j] + solve(i-1,j,grid,dp);

        int left = grid[i][j] + solve(i,j-1,grid,dp);

        return min(up,left);
    }

    int minPathSum(vector<vector<int>>& grid)
    {
        int m = grid.size();
        int n = grid[0].size();

        vector<vector<int>> dp(m, vector<int>(n,-1));

        return solve(m-1,n-1,grid,dp);
    }
};
```

---

# Formula Used While Filling Every Cell

```cpp
if(i==0 && j==0)
dp[i][j]=grid[i][j];

else
dp[i][j] = grid[i][j] + min(Top, Left)
```

---

# Approach 3 — Tabulation

# Tabulation Code

```cpp
class Solution {
public:

    int minPathSum(vector<vector<int>>& grid)
    {
        int m = grid.size();
        int n = grid[0].size();

        vector<vector<int>> dp(m,vector<int>(n,0));

        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(i==0 && j==0)
                {
                    dp[i][j]=grid[i][j];
                    continue;
                }

                int up = 1e9;
                int left = 1e9;

                if(i>0)
                    up = grid[i][j] + dp[i-1][j];

                if(j>0)
                    left = grid[i][j] + dp[i][j-1];

                dp[i][j]=min(up,left);
            }
        }
        return dp[m-1][n-1];
    }
};
```

---

# Space Optimization

# Space Optimized Code

```cpp
class Solution {
public:

    int minPathSum(vector<vector<int>>& grid)
    {
        int m = grid.size();
        int n = grid[0].size();

        vector<int> prev(n,0);

        for(int i=0;i<m;i++)
        {
            vector<int> curr(n,0);

            for(int j=0;j<n;j++)
            {
                if(i==0 && j==0)
                {
                    curr[j]=grid[i][j];
                    continue;
                }

                int up = 1e9;
                int left = 1e9;

                if(i>0)
                    up =
                    grid[i][j]
                    +
                    prev[j];

                if(j>0)
                    left =
                    grid[i][j]
                    +
                    curr[j-1];

                curr[j]=min(up,left);
            }

            prev=curr;
        }

        return prev[n-1];
    }
};
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