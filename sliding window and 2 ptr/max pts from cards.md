# LeetCode 1423 - Maximum Points You Can Obtain from Cards

---

# Problem Statement

There are several cards arranged in a row.

You are given:

- An integer array `cardPoints`
- An integer `k`

In one move, you can pick **one card** from either

- the **beginning**, or
- the **end**

of the array.

You must pick **exactly `k` cards**.

Return the **maximum score** you can obtain.

---

# Example 1

```text
Input

cardPoints = [1,2,3,4,5,6,1]

k = 3
```

Output

```text
12
```

Explanation

Choose

```text
6 + 5 + 1 = 12
```

---

# Why are the Remaining Cards Contiguous?

Suppose

```
1 2 3 4 5 6 1
```

Pick

```
1

2

6
```

Remaining

```
3 4 5
```

Continuous.

---

Pick

```
1

6

5
```

Remaining

```
2 3 4
```

Continuous.

---

Pick

```
6

5

1
```

Remaining

```
1 2 3 4
```

Continuous.

No matter how we pick from the ends,

the remaining cards always form one continuous subarray.

This is the biggest observation.

---

# Main Idea

Instead of finding

```text
Maximum Sum of k picked cards
```

find

```text
Minimum Sum of n-k continuous cards
```

Then

```text
Maximum Score = Total Sum - Minimum Window Sum
```

---

# Approaches

---

# 1. Brute Force

## Intuition

Try every possible way of picking cards.

Pick

```
0
```

from left,

```
k
```

from right.

Then

```
1
```

from left,

```
k-1
```

from right.

Continue until

```
k
```

from left.

Find maximum.

---

# Algorithm

1. Try every split.
2. Compute score.
3. Update maximum.
4. Return answer.

---

# Complexity

Time

```text
O(k²)
```

Space

```text
O(1)
```

---

# Better Approach

Precompute Prefix Sum and Suffix Sum.

For every split,

calculate

```
Left Prefix

+

Right Suffix
```

Take maximum.

---

# Complexity

Time

```text
O(k)
```

Space

```text
O(k)
```

---

# Optimal Approach (Sliding Window using Variables)

⭐ Most Interview Preferred

---

# Intuition

Suppose we need to pick exactly

```text
k
```

cards.

Initially,

pick **all k cards from the left**.

Then,

one by one,

move one card from the **left side** to the **right side**.

For every move,

calculate the score and keep the maximum.

This way, we check every possible combination of

```text
Left Cards + Right Cards
```

without using prefix/suffix arrays.

---

# Algorithm

1. Take the first `k` cards.
2. Store their sum in `leftSum`.
3. Initialize `rightSum = 0`.
4. Answer = `leftSum`.
5. Remove one card from the left.
6. Add one card from the right.
7. Update the answer.
8. Continue until all possibilities are checked.

---

# Optimal Code 

```cpp
class Solution {
public:

    int maxScore(vector<int>& cardPoints, int k) {

        int n = cardPoints.size();

        // Sum of first k cards
        int leftSum = 0;

        for(int i = 0; i < k; i++)
            leftSum += cardPoints[i];

        // Initially answer is taking all cards from left
        int maxScore = leftSum;

        int rightSum = 0;

        // Move one card from left to right
        for(int i = 0; i < k; i++) {

            // Remove one card from the left part
            leftSum -= cardPoints[k - 1 - i];

            // Add one card from the right end
            rightSum += cardPoints[n - 1 - i];

            // Update maximum score
            maxScore = max(maxScore, leftSum + rightSum);
        }

        return maxScore;
    }
};
```

---

# Time Complexity

```text
O(k)
```

---

# Space Complexity

```text
O(1)
```

No extra arrays are used.

---