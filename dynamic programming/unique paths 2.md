# LeetCode 63 - Unique Paths II

---

# Problem Statement

A robot is initially located at the **top-left corner** of an `m x n` grid.
The robot wants to reach the **bottom-right corner**.

The robot can move only:
- Right
- Down

Some cells contain **obstacles**.

```text
0 → Empty Cell
1 → Obstacle
```

The robot **cannot** move through an obstacle.
Return the **number of unique paths** from the start to the destination.

---

## Example

```text
Input

0 0 0

0 1 0

0 0 0
```

Output

```text
2
```

There are only two valid paths because the middle cell is blocked.

---

# Intuition

Suppose we want to compute

```text
dp[i][j]
```

There are only two possible ways to reach this cell.

```
Top

↓

(i-1,j)
```

OR

```
Left

→

(i,j-1)
```

If the current cell is blocked,

there is **no valid path**.

Otherwise,

```text
Paths = Top + Left
```

---

# DP State

```cpp
dp[i][j]
```

Meaning

> Number of unique paths from `(0,0)` to `(i,j)`.

---

# Recurrence

If current cell is blocked

```cpp
dp[i][j]=0;
```

Else

```cpp
dp[i][j] = dp[i-1][j] + dp[i][j-1]
```

---

# Approach 1 — Pure Recursion

## Base Cases

Reached start

```cpp
if(i==0 && j==0)
return 1;
```

---

Outside grid

```cpp
if(i<0 || j<0)
return 0;
```

---

Obstacle

```cpp
if(obstacleGrid[i][j]==1)
return 0;
```

---

# Recursive Code

```cpp
class Solution {
public:

    int solve(int i, int j, vector<vector<int>>& grid)
    {
        if(i >= 0 && j >= 0 && grid[i][j] == 1)
            return 0;

        if(i == 0 && j == 0)
            return 1;

        if(i < 0 || j < 0)
            return 0;

        int up = solve(i-1,j,grid);
        int left = solve(i,j-1,grid);
        return up + left;
    }

    int uniquePathsWithObstacles(vector<vector<int>>& grid)
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
Number of paths
to

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
        if(i >= 0 && j >= 0 && grid[i][j] == 1)
            return 0;

        if(i == 0 && j == 0)
            return 1;

        if(i < 0 || j < 0)
            return 0;

        if(dp[i][j] != -1)
            return dp[i][j];

        int up = solve(i-1,j,grid,dp);
        int left = solve(i,j-1,grid,dp);
        return dp[i][j] = up + left;
    }

    int uniquePathsWithObstacles(vector<vector<int>>& grid)
    {
        int m = grid.size();
        int n = grid[0].size();

        vector<vector<int>> dp(m, vector<int>(n,-1));
        return solve(m-1,n-1,grid,dp);
    }
};
```

--- 

# Special Case

Suppose

```text
1 0

0 0
```

Start itself is blocked.

Answer

```
0
```

---

Suppose

```text
0 0

0 1
```

Destination blocked.

Answer

```
0
```

---

# Approach 3 — Tabulation

## Algorithm

```
If obstacle

dp=0

Else

Top

+

Left
```

---

# Tabulation Code

```cpp
class Solution {
public:

    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid)
    {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();

        vector<vector<int>> dp(
            m,
            vector<int>(n,0)
        );

        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(obstacleGrid[i][j]==1)
                {
                    dp[i][j]=0;
                    continue;
                }

                if(i==0 && j==0)
                {
                    dp[i][j]=1;
                    continue;
                }

                int up=0;
                int left=0;

                if(i>0)
                    up=dp[i-1][j];

                if(j>0)
                    left=dp[i][j-1];

                dp[i][j]=up+left;
            }
        }

        return dp[m-1][n-1];
    }
};
```

---

# Space Optimization

Observation

Current row depends only on

- Previous row
- Current row's previous column

So

```
O(m×n)

↓

O(n)
```

---

# Space Optimized Code

```cpp
class Solution {
public:

    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid)
    {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();

        vector<int> prev(n,0);

        for(int i=0;i<m;i++)
        {
            vector<int> curr(n,0);

            for(int j=0;j<n;j++)
            {
                if(obstacleGrid[i][j]==1)
                {
                    curr[j]=0;
                    continue;
                }

                if(i==0 && j==0)
                {
                    curr[j]=1;
                    continue;
                }

                int up=0;
                int left=0;

                if(i>0)
                    up=prev[j];

                if(j>0)
                    left=curr[j-1];

                curr[j]=up+left;
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