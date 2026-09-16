# LeetCode 213 - House Robber II

---

# Problem Statement

You are a professional robber planning to rob houses along a street.

Each house contains a certain amount of money.

Unlike **House Robber I**, the houses are arranged in a **circle**.

This means:

- The **first house** and the **last house** are adjacent.
- You **cannot rob two adjacent houses**.

Return the **maximum amount of money** you can rob.

---

## Example 1

```text
Input

nums = [2,3,2]

Output

3
```

Explanation

```
Cannot rob

House 1

and

House 3

because they are adjacent.

Best answer

=

3
```

---

## Example 2

```text
Input

nums = [1,2,3,1]

Output

4
```

Explanation

```
Rob

House 2

and

House 4

Total

=

4
```

---

## Example 3

```text
Input

nums = [1,2,3]

Output

3
```

---

# What's Different from House Robber I?

In House Robber I,

houses are arranged in a straight line.

```
1 ---- 2 ---- 3 ---- 4
```

The first and last houses are **not connected**.

---

In House Robber II,

houses form a circle.

```
      1

   /     \

4           2

   \     /

      3
```

Now,

House 1

and

House N

are adjacent.

---

# Key Observation

The first house and last house **cannot be robbed together**.

Therefore, every valid solution belongs to one of these cases.

---

# Case 1

Rob from

```
House 0 to House n-2
```

Ignore the last house.

```
[2,3,2]

↓

[2,3]
```

---

# Case 2

Rob from

```
House 1 to House n-1
```

Ignore the first house.

```
[2,3,2]

↓

[3,2]
```

---

The final answer is

```
max( Case1, Case2 )
```

---

# State Definition

Exactly the same as House Robber I.

```
dp[i] = Maximum money that can be robbed
up to house i.
```

---

# Approach 1 — Pure Recursion

## Idea

Create a recursive function identical to

House Robber I.

Run it

twice.

---

# Recursive Code

```cpp
class Solution {
public:

    int solve(int index, vector<int>& nums)
    {
        if(index == 0)
            return nums[0];

        if(index < 0)
            return 0;

        int pick = nums[index] + solve(index - 2, nums);
        int notPick = solve(index - 1, nums);
        return max(pick, notPick);
    }

    int rob(vector<int>& nums)
    {
        int n = nums.size();

        if(n == 1)
            return nums[0];

        vector<int> first(nums.begin(), nums.end() - 1);
        vector<int> second(nums.begin() + 1, nums.end());

        return max(
            solve(first.size() - 1, first),
            solve(second.size() - 1, second)
        );
    }
};
```

---

# Dry Run (Recursion)

Input

```
nums

[2,3,2]
```

Case 1

```
[2,3]
```

Answer

```
3
```

---

Case 2

```
[3,2]
```

Answer

```
3
```

Final

```
max(3,3)

=

3
```

---

# Approach 2 — Memoization

## Idea

Same memoization used in House Robber I.

The only difference is run it twice.

---

# Memoization Code

```cpp
class Solution {
public:

    int solve(int index,
              vector<int>& nums,
              vector<int>& dp)
    {
        if(index == 0)
            return nums[0];

        if(index < 0)
            return 0;

        if(dp[index] != -1)
            return dp[index];

        int pick = nums[index]
                 + solve(index - 2,
                         nums,
                         dp);

        int notPick = solve(index - 1,
                            nums,
                            dp);

        dp[index] = max(pick, notPick);

        return dp[index];
    }

    int rob(vector<int>& nums)
    {
        int n = nums.size();

        if(n == 1)
            return nums[0];

        vector<int> first(nums.begin(), nums.end() - 1);
        vector<int> second(nums.begin() + 1, nums.end());

        vector<int> dp1(first.size(), -1);
        vector<int> dp2(second.size(), -1);

        return max(
            solve(first.size() - 1, first, dp1),
            solve(second.size() - 1, second, dp2)
        );
    }
};
```

---

# Dry Run (Memoization)

Input

```
nums

[1,2,3,1]
```

Case 1

```
[1,2,3]
```

DP

```
[1,2,4]
```

Answer

```
4
```

---

Case 2

```
[2,3,1]
```

DP

```
[2,3,3]
```

Answer

```
3
```

Final

```
max(

4,

3

)

=

4
```

---

# Time Complexity

Each memoized solution

```
O(N)
```

Run twice

```
O(2N)

=

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

---

# Helper Function

```
solve(nums)

↓

Returns maximum money
using House Robber I DP.
```

---

# Tabulation Code

```cpp
class Solution {
public:

    int solve(vector<int>& nums)
    {
        int n = nums.size();
        vector<int> dp(n);
        dp[0] = nums[0];

        for(int i = 1; i < n; i++)
        {
            int pick = nums[i];

            if(i > 1)
                pick += dp[i - 2];

            int notPick = dp[i - 1];

            dp[i] = max(pick, notPick);
        }
        return dp[n - 1];
    }

    int rob(vector<int>& nums)
    {
        int n = nums.size();

        if(n == 1)
            return nums[0];

        vector<int> first(nums.begin(), nums.end() - 1);
        vector<int> second(nums.begin() + 1, nums.end());

        return max(solve(first), solve(second));
    }
};
```

---

# DP Table Dry Run

Input

```text
nums = [1,2,3,1]
```

---

## Case 1

Ignore last house

```
[1,2,3]
```

DP Table

| House | DP |
|------|----|
|0|1|
|1|2|
|2|4|

Answer

```
4
```

---

## Case 2

Ignore first house

```
[2,3,1]
```

DP Table

| House | DP |
|------|----|
|0|2|
|1|3|
|2|3|

Answer

```
3
```

---

Final Answer

```
max(4,3)

=

4
```

---

# Time Complexity

Each helper function runs in

```
O(N)
```

We call it twice.

```
O(2N)

=

O(N)
```

---

# Space Complexity

DP Array

```
O(N)
```

---

# Approach 4 — Space Optimization (Optimal)

---

# Complete Optimal Solution

```cpp
class Solution {
public:

    int solve(vector<int>& nums)
    {
        int n = nums.size();

        if(n == 1)
            return nums[0];

        int prev2 = nums[0];
        int prev1 = max(nums[0], nums[1]);

        for(int i = 2; i < n; i++)
        {
            int pick = nums[i] + prev2;

            int notPick = prev1;

            int curr = max(pick, notPick);

            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }

    int rob(vector<int>& nums)
    {
        int n = nums.size();

        if(n == 1)
            return nums[0];

        vector<int> first(nums.begin(), nums.end() - 1);
        vector<int> second(nums.begin() + 1, nums.end());

        return max(
            solve(first),
            solve(second)
        );
    }
};
```

---

# Dry Run (Optimal)

Input

```text
nums = [2,3,2]
```

---

Case 1

```
[2,3]
```

```
prev2 = 2

prev1 = 3
```

Answer

```
3
```

---

Case 2

```
[3,2]
```

```
prev2 = 3

prev1 = 3
```

Answer

```
3
```

---

Final

```
max(3,3)

=

3
```

---

```cpp
return max(solve(first), solve(second));
```

Solve House Robber I on both arrays.

Return the larger answer.

---

Inside helper:

```cpp
prev2 = nums[0];
```

Best answer up to the first house.

---

```cpp
prev1 = max(nums[0], nums[1]);
```

Best answer up to the second house.

---

```cpp
pick = nums[i] + prev2;
```

Rob the current house.

---

```cpp
notPick = prev1;
```

Skip the current house.

---

```cpp
curr = max(pick, notPick);
```

Choose the better option.

---

```cpp
prev2 = prev1;

prev1 = curr;
```

Move the DP window forward.

---

# Complexity Comparison

| Approach | Time | Space |
|----------|------|-------:|
| Recursion | O(2ᴺ) | O(N) |
| Memoization | O(N) | O(N) |
| Tabulation | O(N) | O(N) |
| Space Optimized | O(N) | O(1)* |

> *The helper function itself uses O(1) extra space. In this implementation, creating the two temporary arrays costs O(N). If you instead pass index ranges (e.g., `start` and `end`) to the helper, the entire solution can be made O(1) extra space.*

---