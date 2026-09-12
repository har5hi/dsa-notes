# Frog Jump with K Steps | Dynamic Programming

---

# Problem Statement

A frog is standing at the **0th stair** and wants to reach the **(N-1)th stair**.
Each stair has a height given in the array `heights[]`.
The frog can jump **at most K stairs** in one move.
The energy consumed while jumping from stair `i` to stair `j` is:

```text
abs(heights[i] - heights[j])
```

Return the **minimum total energy** required to reach the last stair.

---

## Example

```text
Input

N = 5
K = 3
heights = [10,30,40,50,20]

Output

30
```

### Explanation

Optimal Path

```
0 → 1 → 4

Energy
|10-30| = 20
|30-20| = 10
Total = 30
```

---

# Intuition

In the normal Frog Jump problem, the frog had only **2 choices**:

- Jump 1 stair
- Jump 2 stairs

Now,the frog can jump

```
1
2
3
...
K
```

stairs.

Instead of checking only the previous one or two stairs, we now check the **previous K stairs**.

---

# State Definition

Let

```
dp[i]
```

represent

```
Minimum energy required to reach stair i.
```

---

# Recurrence Relation

For every possible jump

```
j = 1 → K
```

```
dp[i] = minimum(dp[i-j] + abs(height[i]-height[i-j]))
```

provided

```
i-j >= 0
```

---

# Approach 1 — Pure Recursion

## Idea

To reach stair `index`, the frog could have come from

```
index-1
index-2
...

index-K
```

Compute every possibility recursively and return the minimum.

---

## Base Case

```
index == 0
return 0
```

---

# Recursive Code

```cpp
class Solution {
public:

    int solve(int index, vector<int>& heights, int k)
    {
        if(index == 0)
            return 0;

        int ans = INT_MAX;

        for(int jump = 1; jump <= k; jump++)
        {
            if(index - jump >= 0)
            {
                int cost = solve(index - jump, heights, k) + abs(heights[index] - heights[index - jump]);
                ans = min(ans, cost);
            }
        }
        return ans;
    }

    int frogJump(int n, vector<int>& heights, int k)
    {
        return solve(n - 1, heights, k);
    }
};
```

---

# Time Complexity

Every state branches into

```
K
```

recursive calls.

Approximately

```
O(K^N)
```

---

# Space Complexity

```
O(N)
```

Recursive stack.

---

# Why Memoization?

The same states

```
solve(4)

solve(3)

solve(2)
```

are solved many times.

Store the answers after computing them once.

---

# Approach 2 — Memoization

## Idea

Use

```
dp[index]
```

to store the answer.

If already computed, return it immediately.

---

# Memoization Code

```cpp
class Solution {
public:

    int solve(int index, vector<int>& heights,
              vector<int>& dp, int k)
    {
        if(index == 0)
            return 0;

        if(dp[index] != -1)
            return dp[index];

        int ans = INT_MAX;

        for(int jump = 1; jump <= k; jump++)
        {
            if(index - jump >= 0)
            {
                int cost = solve(index - jump, heights, dp, k) + abs(heights[index] - heights[index - jump]);
                ans = min(ans, cost);
            }
        }

        dp[index] = ans;
        return ans;
    }

    int frogJump(int n, vector<int>& heights, int k)
    {
        vector<int> dp(n, -1);
        return solve(n - 1, heights, dp, k);
    }
};
```

---

# Time Complexity

Each state is computed once.

For every state, we check

```
K
```

jumps.

```
O(N × K)
```

---

# Space Complexity

DP Array

```
O(N)
```

Recursive Stack

```
O(N)
```

Total

```
O(N)
```

---

# Approach 3 — Tabulation

## Idea

Instead of recursion, build the answer iteratively.

Start from

```
dp[0]
```

and calculate all remaining states.

---

# Tabulation Code

```cpp
class Solution {
public:

    int frogJump(int n, vector<int>& heights, int k)
    {
        vector<int> dp(n, 0);

        dp[0] = 0;

        for(int i = 1; i < n; i++)
        {
            int ans = INT_MAX;

            for(int jump = 1; jump <= k; jump++)
            {
                if(i - jump >= 0)
                {
                    int cost = dp[i - jump] + abs(heights[i] - heights[i - jump]);
                    ans = min(ans, cost);
                }
            }
            dp[i] = ans;
        }
        return dp[n - 1];
    }
};
```

---

# Time Complexity

Outer Loop

```
N
```

Inner Loop

```
K
```

Total

```
O(N × K)
```

---

# Space Complexity

```
O(N)
```

---

# Can We Space Optimize?

❌ **No (in general).**

Unlike the normal Frog Jump problem, where each state depends only on the previous **two** states,

```
dp[i]

depends on

dp[i-1]

dp[i-2]

...

dp[i-K]
```

Since we may need up to **K previous values**, we cannot reduce the DP array to just two variables.

Therefore, the standard solution uses the full DP array.

---

# Complexity Comparison

| Approach | Time | Space |
|----------|------|-------|
| Recursion | O(Kᴺ) | O(N) |
| Memoization | O(N × K) | O(N) |
| Tabulation | O(N × K) | O(N) |

---

# Pattern Recognition

Whenever you notice:

- Minimum cost
- Variable jump lengths
- Current answer depends on multiple previous states

Think of **1D Dynamic Programming with a loop over previous states**.

Similar problems include:

- Frog Jump with K Distance
- Minimum Cost Climbing
- Coin Change (state transition over multiple choices)
- Rod Cutting

---