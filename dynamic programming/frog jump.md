# Frog Jump (Minimum Energy) | Dynamic Programming

 
---

# Problem Statement

A frog is standing at the **0th stair** and wants to reach the **(N-1)th stair**.

Each stair has a height given in the array `heights[]`.

The frog can jump:

- Exactly **1 stair**
- Exactly **2 stairs**

The energy consumed while jumping from stair `i` to stair `j` is:

```text
abs(heights[i] - heights[j])
```

Return the **minimum total energy** required to reach the last stair.

---

## Example

```text
Input

heights = [30,10,60,10,60,50]

Output

40
```

### Explanation

```
0 → 2 → 4 → 5

Energy

|30-60| = 30

|60-60| = 0

|60-50| = 10

Total = 40
```

---

# Intuition

At every stair, the frog has only **two choices**:

- Jump 1 stair
- Jump 2 stairs

We want the **minimum energy**, not the number of ways.

Suppose we are standing at stair `i`.

To reach this stair, we could have come from:

- stair `i-1`
- stair `i-2`

Therefore,

```
Minimum Energy(i) = minimum(Energy(i-1)+cost(i-1,i),Energy(i-2)+cost(i-2,i))
```

This naturally leads to Dynamic Programming.

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

```
dp[i] = min(dp[i-1] + abs(height[i]-height[i-1]),dp[i-2] + abs(height[i]-height[i-2]))
```

---

# Approach 1 — Pure Recursion

## Idea

Think backwards.

To reach stair `i`, the frog must have come from:

- `i-1` 
- `i-2`

Solve both possibilities recursively and return the smaller answer.

---

## Base Case

When

```
index == 0
```

The frog is already at the starting stair.

No energy is required.

Return

```
0
```

---

# Recursive Code

```cpp
class Solution {
public:

    int solve(int index, vector<int>& heights)
    {
        if(index == 0)
            return 0;

        int left = solve(index - 1, heights) + abs(heights[index] - heights[index - 1]);

        int right = INT_MAX;

        if(index > 1)
        {
            right = solve(index - 2, heights) + abs(heights[index] - heights[index - 2]);
        }
        return min(left, right);
    }

    int frogJump(int n, vector<int>& heights)
    {
        return solve(n - 1, heights);
    }
};
```

---

# Time Complexity

Each call branches into two recursive calls.

```
O(2^N)
```

---

# Space Complexity

Recursive stack

```
O(N)
```

---

# Why Recursion is Slow

For

```
N = 40
```

States like

```
solve(20)

solve(19)

solve(18)
```

are solved many times.

Lots of repeated work.

Solution: Store the answer after computing it once. This is called **Memoization**.

---

# Approach 2 — Memoization (Top-Down DP)

## Idea

Whenever we compute

```
solve(index)
```

Store the answer inside

```
dp[index]
```

If we need the same state again, return it immediately.

---

# DP Array

```
dp[i] = Minimum energy needed to reach stair i
```

Initially

```
[-1,-1,-1,-1,-1...]
```

---

# Memoization Code

```cpp
class Solution {
public:

    int solve(int index, vector<int>& heights, vector<int>& dp)
    {
        if(index == 0)
            return 0;

        if(dp[index] != -1)
            return dp[index];

        int left = solve(index - 1, heights, dp) + abs(heights[index] - heights[index - 1]);

        int right = INT_MAX;

        if(index > 1)
        {
            right = solve(index - 2, heights, dp) + abs(heights[index] - heights[index - 2]);
        }

        dp[index] = min(left, right);
        return dp[index];
    }

    int frogJump(int n, vector<int>& heights)
    {
        vector<int> dp(n, -1);
        return solve(n - 1, heights, dp);
    }
};
```

---

# Time Complexity

Each state is computed only once.

```
O(N)
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

# Approach 3 — Tabulation (Bottom-Up DP)

## Idea

Instead of solving the problem recursively from the last stair,

start from the **smallest subproblem** and build the answer iteratively.

We already know:

```
dp[i] = Minimum energy required to reach stair i
```

We compute the answer from

```
0 → n-1
```

---

# DP Definition

```
dp[i] = Minimum energy required to reach stair i.
```

---

# Base Case

The frog is already standing on the first stair.

No energy is needed.

```
dp[0] = 0
```

---

# Transition

To reach stair `i`, there are only two possibilities.

### Jump from previous stair

```
left = dp[i-1] + abs(height[i]-height[i-1])
```

---

### Jump from two stairs behind

```
right = dp[i-2] + abs(height[i]-height[i-2])
```

---

Take the minimum.

```
dp[i] = min(left,right)
```

---

# Tabulation Code

```cpp
class Solution {
public:

    int frogJump(int n, vector<int>& heights)
    {
        vector<int> dp(n, 0);
        dp[0] = 0;

        for(int i = 1; i < n; i++)
        {
            int left = dp[i - 1] + abs(heights[i] - heights[i - 1]);
            int right = INT_MAX;

            if(i > 1)
            {
                right = dp[i - 2] + abs(heights[i] - heights[i - 2]);
            }
            dp[i] = min(left, right);
        }
        return dp[n - 1];
    }
};
```

---

# Time Complexity

Single loop

```
O(N)
```

---

# Space Complexity

DP Array

```
O(N)
```

---

# Approach 4 — Space Optimization

## Observation

Notice the recurrence:

```
dp[i] = min( dp[i-1]+cost, dp[i-2]+cost)
```

We only use

```
dp[i-1] and dp[i-2]
```

The remaining DP array is never used again.

Therefore, instead of storing the entire array, store only the previous two answers.

---

# Space Optimized Code

```cpp
class Solution {
public:

    int frogJump(int n, vector<int>& heights)
    {
        int prev2 = 0;
        int prev1 = 0;

        for(int i = 1; i < n; i++)
        {
            int left = prev1 + abs(heights[i] - heights[i - 1]);

            int right = INT_MAX;

            if(i > 1)
            {
                right = prev2 + abs(heights[i] - heights[i - 2]);
            }

            int curr = min(left, right);
            prev2 = prev1;
            prev1 = curr;
        }
        return prev1;
    }
};
```

---

# Complexity Comparison

| Approach | Time | Space |
|----------|------|-------:|
| Recursion | O(2ᴺ) | O(N) |
| Memoization | O(N) | O(N) |
| Tabulation | O(N) | O(N) |
| Space Optimized | O(N) | O(1) |

---

# Pattern Recognition

Whenever you notice:

- Minimum / Maximum cost
- Reach the last index
- Fixed jump choices (1 step, 2 steps)
- Current answer depends on previous states

Think of **1D Dynamic Programming**.

Common problems following the same pattern:

- Climbing Stairs
- Min Cost Climbing Stairs
- House Robber
- Frog Jump (Minimum Energy)

---

# Interview Tips

✅ Start by writing the recurrence relation.

✅ Explain why there are only two possible previous states.

✅ Begin with recursion to derive the solution.

✅ Improve it using memoization.

✅ Convert it into tabulation.

✅ Observe that only the previous two DP states are needed and optimize the space to **O(1)**.

Interviewers often expect candidates to arrive at the **space-optimized solution**.

---