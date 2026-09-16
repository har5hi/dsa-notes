# LeetCode 198 - House Robber

---

# Problem Statement

You are a professional robber planning to rob houses along a street.

Each house contains a certain amount of money.

The only constraint is:

> **You cannot rob two adjacent houses**, because the security system will automatically alert the police.

Return the **maximum amount of money** you can rob without robbing two adjacent houses.

---

## Example 1

```text
Input:

nums = [1,2,3,1]

Output:

4
```

Explanation

```
Rob House 1

Money = 1

Rob House 3

Money = 3

Total = 4
```

---

## Example 2

```text
Input

nums = [2,7,9,3,1]

Output

12
```

Explanation

```
Rob House 1 = 2

Rob House 3 = 9

Rob House 5 = 1

Total = 12
```

---

# Intuition

At every house, there are only **two choices**.

### Choice 1 — Rob this house

If we rob the current house,

we **cannot rob the previous house**.

So we move to

```
index - 2
```

---

### Choice 2 — Skip this house

Don't rob it.

Move to

```
index - 1
```

---

Finally,

take the better option.

```
Maximum Money

=

max(

Rob Current House,

Skip Current House

)
```

---

# State Definition

Let

```
dp[i]
```

represent

```
Maximum money that can be robbed
from House 0 to House i.
```

---

# Recurrence Relation

If we rob

```
House i
```

then

```
Take = nums[i] + dp[i-2]
```

If we don't rob

```
House i
```

then

```
Not Take = dp[i-1]
```

Therefore

```
dp[i] = max(Take, Not Take)
```

---

# Approach 1 — Pure Recursion

## Idea

Think backwards.

Suppose we are standing at

```
House i
```

We have only two choices.

### Pick

```
nums[i] + solve(i-2)
```

---

### Not Pick

```
solve(i-1)
```

Return

```
max(pick,notPick)
```

---

# Base Cases

If

```
index==0
```

Only one house exists.

Rob it.

Return

```
nums[0]
```

---

If

```
index<0
```

No house left.

Return

```
0
```

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
        return solve(nums.size() - 1, nums);
    }
};
```

---

# Recursive Tree

Example

```
nums

[2,7,9,3]
```

```
                 solve(3)

              /            \

         solve(1)        solve(2)

        /      \        /       \

    solve(-1) solve(0) solve(0) solve(1)
```

Notice

```
solve(1)

solve(0)
```

are computed repeatedly.

---

# Dry Run (Recursion)

Input

```
nums

[2,7,9,3]
```

Need

```
solve(3)
```

```
Pick

=

3

+

solve(1)
```

```
Not Pick

=

solve(2)
```

Again,

```
solve(1)
```

gets called from multiple places.

Lots of repeated work.

---

# Time Complexity

Each call creates two recursive calls.

```
O(2^N)
```

---

# Space Complexity

Recursive Stack

```
O(N)
```

---

# Approach 2 — Memoization (Top-Down DP)

## Idea

Whenever

```
solve(index)
```

is computed, store it inside

```
dp[index]
```

Next time, return it immediately.

---

# DP Array

```
dp[i] = Maximum money till house i
```

Initially

```
[-1,-1,-1,-1...]
```

---

# Memoization Code

```cpp
class Solution {
public:

    int solve(int index, vector<int>& nums, vector<int>& dp)
    {
        if(index == 0)
            return nums[0];

        if(index < 0)
            return 0;

        if(dp[index] != -1)
            return dp[index];

        int pick = nums[index] + solve(index - 2, nums, dp);
        int notPick = solve(index - 1, nums, dp);
        dp[index] = max(pick, notPick);
        return dp[index];
    }

    int rob(vector<int>& nums)
    {
        int n = nums.size();
        vector<int> dp(n, -1);
        return solve(n - 1, nums, dp);
    }
};
```

---

# Dry Run (Memoization)

Input

```
nums

[2,7,9,3,1]
```

Initially

```
dp

[-1,-1,-1,-1,-1]
```

After solving

```
dp

[2,7,11,11,12]
```

Meaning

```
House0

Best =2
```

```
House1

Best =7
```

```
House2

Best =11
```

```
House3

Best =11
```

```
House4

Best =12
```

Now if

```
solve(2)
```

is needed again,

we simply return

```
dp[2]
```

instead of solving it again.

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

Instead of solving recursively, build the answer from the smallest subproblem.

We already know

```
dp[i] = Maximum money that can be robbed from House 0 to House i.
```

Start from

```
House 0
```

and build the answer until

```
House n-1
```

---

# Base Case

Only one house exists.

```
dp[0]=nums[0]
```

---

# Transition

## Option 1 — Pick Current House

If we rob the current house, we cannot rob the previous one.

```
pick = nums[i] + dp[i-2]
```

If

```
i==1
```

then

```
dp[-1]
```

doesn't exist, so

```
pick=nums[i]
```

---

## Option 2 — Don't Pick Current House

Skip this house.

```
notPick = dp[i-1]
```

---

Take the better option.

```
dp[i] = max(pick,notPick)
```

---

# Tabulation Code

```cpp
class Solution {
public:

    int rob(vector<int>& nums)
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
};
```

---

# DP Table Dry Run

Input

```text
nums = [2,7,9,3,1]
```

Initially

| House | DP |
|------|----|
|0|2|
|1|0|
|2|0|
|3|0|
|4|0|

---

## House 1

```
Pick

=

7
```

```
Not Pick

=

2
```

Maximum

```
7
```

| House | DP |
|------|----|
|0|2|
|1|7|
|2|0|
|3|0|
|4|0|

---

## House 2

```
Pick

=

9+2

=11
```

```
Not Pick

=

7
```

Maximum

```
11
```

| House | DP |
|------|----|
|0|2|
|1|7|
|2|11|
|3|0|
|4|0|

---

## House 3

```
Pick

=

3+7

=10
```

```
Not Pick

=

11
```

Maximum

```
11
```

| House | DP |
|------|----|
|0|2|
|1|7|
|2|11|
|3|11|
|4|0|

---

## House 4

```
Pick

=

1+11

=12
```

```
Not Pick

=

11
```

Maximum

```
12
```

Final DP

| House | 0 | 1 | 2 | 3 | 4 |
|------|---|---|---|---|---|
| DP | 2 | 7 | 11 | 11 | 12 |

Answer

```
12
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

We don't need the entire DP array.
Store only the previous two values.

---

# Space Optimized Code

```cpp
class Solution {
public:

    int rob(vector<int>& nums)
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
};
```

---

# Dry Run (Space Optimized)

Input

```text
nums = [2,7,9,3,1]
```

Initially

```
prev2 = 2

prev1 = 7
```

---

### House 2

```
pick

=

9+2

=11
```

```
notPick

=

7
```

```
curr

=

11
```

Update

```
prev2 = 7

prev1 = 11
```

---

### House 3

```
pick

=

3+7

=10
```

```
notPick

=

11
```

```
curr

=

11
```

Update

```
prev2 = 11

prev1 = 11
```

---

### House 4

```
pick

=

1+11

=12
```

```
notPick

=

11
```

```
curr

=

12
```

Update

```
prev2 = 11

prev1 = 12
```

Return

```
12
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

Whenever you see:

- Choose or skip an element
- Adjacent elements cannot both be chosen
- Maximum / Minimum sum
- Current decision depends on the previous two states

Think of **Pick / Not Pick Dynamic Programming**.

---