# LeetCode 70 - Climbing Stairs

---

# Problem Statement

You are climbing a staircase.

It takes **n** steps to reach the top.

Each time you can either climb:

- 1 step
- 2 steps

Return the number of distinct ways to reach the top.

### Example 1

```text
Input: n = 2
Output: 2

Explanation:
1 + 1
2
```

### Example 2

```text
Input: n = 3
Output: 3

Explanation:
1 + 1 + 1
1 + 2
2 + 1
```

---

# Intuition

At every stair, there are only **two possible choices**:

- Take **1 step**
- Take **2 steps**

Suppose you're standing on stair `i`.

To reach stair `i`, you must have come from:

- stair `i-1`
- stair `i-2`

So,

```
Ways(i) = Ways(i-1) + Ways(i-2)
```

This is exactly the **Fibonacci sequence**.

---

# Approach 1 — Pure Recursion

## Idea

Think recursively.

Suppose we need the number of ways to climb `n` stairs.

We have two choices:

- climb 1 stair → remaining = n-1
- climb 2 stairs → remaining = n-2

So,

```
f(n) = f(n-1) + f(n-2)
```

---

## Base Cases

If

```
n == 0
```

We've exactly reached the top.

Return **1**.

If

```
n < 0
```

We crossed the stairs.

Return **0**.

---

## Recursive Tree (n = 4)

```
                 f(4)
              /       \
           f(3)       f(2)
          /   \       /   \
       f(2) f(1)   f(1) f(0)
      /   \
   f(1) f(0)
```

Notice how

```
f(2)
```

is calculated multiple times.

This is called **overlapping subproblems**.

---

## Recursive Code

```cpp
class Solution {
public:

    int climb(int n)
    {
        if(n == 0)
            return 1;

        if(n < 0)
            return 0;

        return climb(n-1) + climb(n-2);
    }

    int climbStairs(int n) {
        return climb(n);
    }
};
```

---

## Time Complexity

Every call creates two more calls.

```
O(2^n)
```

---

## Space Complexity

Recursive stack depth

```
O(n)
```

---

# Why Recursion is Slow

For

```
n = 40
```

the function recalculates

```
climb(30)
climb(29)
climb(28)
```

thousands of times.

Lots of repeated work.

Solution: Store answers after calculating them once.

This is called **Memoization**.

---

# Approach 2 — Memoization (Top Down DP)

## Idea

Whenever we calculate

```
climb(5)
```

store its answer.
Next time if we need it, don't calculate again.
Simply return it.

---

## DP Array

```
dp[i]
```

stores

```
Number of ways to reach stair i
```

Initially

```
dp = [-1,-1,-1,-1,-1...]
```

---

## Algorithm

```
climb(n)

if n==0 return 1

if n<0 return 0

if dp[n]!=-1
    return dp[n]

dp[n]=climb(n-1)+climb(n-2)

return dp[n]
```

---

## Memoization Code

```cpp
class Solution {
public:

    int climb(int n, vector<int>& dp)
    {
        if(n == 0)
            return 1;

        if(n < 0)
            return 0;

        if(dp[n] != -1)
            return dp[n];

        dp[n] = climb(n-1, dp) + climb(n-2, dp);

        return dp[n];
    }

    int climbStairs(int n) {

        vector<int> dp(n+1, -1);

        return climb(n, dp);
    }
};
```

## Time Complexity

Each state computed once.

```
O(n)
```

---

## Space Complexity

DP array

```
O(n)
```

Recursive stack

```
O(n)
```

Total

```
O(n)
```

---

# Approach 3 — Tabulation (Bottom Up DP)

## Idea

Instead of starting from

```
n
```

and going downward, start from the smallest subproblem.

Build answers from

```
0 → n
```

---

## DP Definition

```
dp[i] = Number of ways to reach stair i
```

---

## Base Values

```
dp[0]=1
dp[1]=1
```

Why?

There is exactly **1 way** to: 

Reach stair 0 → Do nothing.
Reach stair 1 → Take one step.

---

## Transition

```
dp[i]=dp[i-1]+dp[i-2]
```

---

## DP Table (n = 5)

| i | dp[i] |
|---|-------|
|0|1|
|1|1|
|2|2|
|3|3|
|4|5|
|5|8|

---

## Visual Representation

```
dp[0]=1

dp[1]=1

dp[2]=1+1=2

dp[3]=2+1=3

dp[4]=3+2=5

dp[5]=5+3=8
```

---

## Tabulation Code

```cpp
class Solution {
public:
    int climbStairs(int n) {

        vector<int> dp(n + 1);

        dp[0] = 1;
        dp[1] = 1;

        for(int i = 2; i <= n; i++)
        {
            dp[i] = dp[i-1] + dp[i-2];
        }

        return dp[n];
    }
};
```

---

## Time Complexity

Single loop

```
O(n)
```

---

## Space Complexity

DP array

```
O(n)
```

---

# Space Optimized DP

Notice

```
dp[i]
```

only depends on

```
dp[i-1]
```

and

```
dp[i-2]
```

So the whole DP array isn't necessary.
Keep only two variables.

---

## Code

```cpp
class Solution {
public:
    int climbStairs(int n) {

        if(n == 1)
            return 1;

        int prev2 = 1;
        int prev1 = 1;

        for(int i = 2; i <= n; i++)
        {
            int curr = prev1 + prev2;
            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }
};
```

---

## Time Complexity

```
O(n)
```

---

## Space Complexity

```
O(1)
```

---

# Comparison of All Approaches

| Approach | Time | Space | Notes |
|----------|------|-------|------|
| Recursion | O(2^n) | O(n) | Very slow, repeated work |
| Memoization | O(n) | O(n) | Top-down DP |
| Tabulation | O(n) | O(n) | Bottom-up DP |
| Space Optimized | O(n) | O(1) | Best solution |

---

# Pattern Recognition

Whenever you notice:

- Count total ways
- Two or more choices at every step
- Current answer depends on previous states

Think of:

- Fibonacci
- Dynamic Programming

This problem is one of the most important beginner DP problems and introduces the **Top-Down** and **Bottom-Up** dynamic programming patterns.

---

# Interview Tips

✅ Start with recursion to derive the recurrence relation.

✅ Point out the overlapping subproblems.

✅ Convert recursion to memoization by caching results.

✅ Convert memoization to tabulation.

✅ Observe that only the previous two states are needed and optimize the space to **O(1)**.

Interviewers often expect you to reach the space-optimized solution after discussing the DP approaches.

---