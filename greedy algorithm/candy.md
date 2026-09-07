# LeetCode 135 - Candy

## 📌 Problem Statement

There are `n` children standing in a line.

Each child has a rating given in the array `ratings`.

You must distribute candies according to the following rules:

1. Every child must receive **at least one candy**.
2. A child with a **higher rating than an adjacent child** must receive **more candies** than that neighbor.

Return the **minimum number of candies** needed.

---

## Example 1

```text
Input

ratings = [1,0,2]

Output

5
```

---

## Example 2

```text
Input

ratings = [1,2,2]

Output

4
```

---

# 💡 Intuition

Each child depends on **both neighbors**.
Suppose we only scan from left to right.

```
Ratings
1 2 3 2
```

We may correctly assign:

```
1 2 3 1
```

Now consider

```
Ratings
3 2 1
```

A left-to-right scan gives

```
1 1 1
```

which is incorrect.

Similarly, a right-to-left scan alone also fails for increasing sequences.

Therefore, we need **two passes**:

- Left → Right (handle increasing ratings)
- Right → Left (handle decreasing ratings)

The final candies for each child should satisfy **both conditions**, so we take the maximum from the two passes.

---

# Optimal Approach (Greedy - Two Passes)

Create an array

```text
candies[]
```

Initially,

```text
Every child gets 1 candy.
```

---

### First Pass (Left → Right)

If

```text
ratings[i] > ratings[i-1]
```

then

```text
candies[i] = candies[i-1] + 1
```

This satisfies the **left neighbor** condition.

---

### Second Pass (Right → Left)

If

```text
ratings[i] > ratings[i+1]
```

then

```text
candies[i] =
max(candies[i], candies[i+1] + 1)
```

The `max()` is important because the child may already have enough candies from the first pass.

---

Finally,

sum all candies.

---

# C++ Code

```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        int n = ratings.size();
        vector<int> candies(n, 1);

        for (int i = 1; i < n; i++) {
            if (ratings[i] > ratings[i - 1]) {
                candies[i] = candies[i - 1] + 1;
            }
        }

        for (int i = n - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1]) {
                candies[i] = max(candies[i], candies[i + 1] + 1);
            }
        }

        int total = 0;

        for (int candy : candies)
            total += candy;

        return total;
    }
};
```

---

# ⏱ Time Complexity

First pass

```text
O(n)
```

Second pass

```text
O(n)
```

Summation

```text
O(n)
```

Overall

```text
O(n)
```

---

# 📦 Space Complexity

Candy array

```text
O(n)
```

---

# 🎯 Why Greedy Works?

There are **two independent constraints**:

### Constraint 1

Higher rating than the left neighbor.
Handled by

```
Left → Right
```

---

### Constraint 2

Higher rating than the right neighbor.
Handled by

```
Right → Left
```

After the first pass, every left condition is satisfied.
After the second pass,every right condition is satisfied.

Taking

```cpp
max(leftValue, rightValue)
```

ensures both constraints hold simultaneously while using the minimum number of candies.

---

# 🚀 Optimized Approach (O(1) Space)

There is an advanced greedy solution that solves the problem in **O(1)** extra space by tracking:

- Increasing slopes
- Decreasing slopes
- Peak length

However, the logic is significantly more complex.

For interviews, the **two-pass O(n) space solution** is the standard and most commonly expected approach.

---