# LeetCode 55 - Jump Game

## 📌 Problem Statement

You are given an integer array `nums`.

Each element `nums[i]` represents the **maximum jump length** from that position.

Determine whether you can reach the **last index** starting from the first index.

Return:

- `true` if you can reach the last index.
- `false` otherwise.

---

## Example 1

```text
Input

nums = [2,3,1,1,4]

Output

true
```

### Explanation

```
Index: 0 1 2 3 4
Value: 2 3 1 1 4
```

One possible path:

```
0 → 1 → 4
```

Reached the last index.

---

## Example 2

```text
Input

nums = [3,2,1,0,4]

Output

false
```

### Explanation

```
Index: 0 1 2 3 4
Value: 3 2 1 0 4
```

No matter how you jump,

you get stuck at index **3** because

```
nums[3] = 0
```

So index 4 is unreachable.

---

# 💡 Intuition

At every index, we only care about one thing:

> **How far can we reach?**

Suppose we are at index `i`.

From here we can reach

```text
i + nums[i]
```

If this is farther than our previous maximum reach,

update it.

As we move through the array:

- If we ever arrive at an index that is **beyond our maximum reachable position**, we are stuck.
- Otherwise, keep extending the reachable range.

This greedy observation is enough to solve the problem.

---

# Optimal Approach (Greedy)

Maintain the **farthest index** we can currently reach.

Initialize

```text
maxReach = 0
```

Traverse the array.

For every index:

If

```text
i > maxReach
```

we cannot even reach this index.

Return

```text
false
```

Otherwise,

update

```text
maxReach = max(maxReach, i + nums[i])
```

If traversal finishes,

return

```text
true
```

---

# C++ Code

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int maxReach = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (i > maxReach)
                return false;
            maxReach = max(maxReach, i + nums[i]);
        }
        return true;
    }
};
```

---

# ⏱ Time Complexity

Single traversal.

```text
O(n)
```

---

# 📦 Space Complexity

Only one variable.

```text
O(1)
```

---

# 🔍 Line-by-Line Explanation

```cpp
int maxReach = 0;
```

Stores the farthest index we can currently reach.

---

```cpp
for(int i = 0; i < nums.size(); i++)
```

Visit every index.

---

```cpp
if(i > maxReach)
```

Current index is unreachable.

No solution exists.

---

```cpp
return false;
```

Immediately stop.

---

```cpp
maxReach = max(maxReach, i + nums[i]);
```

Update the farthest reachable position.

---

```cpp
return true;
```

Successfully reached every required position.

---

# 🎯 Why Greedy Works?

Suppose

```text
nums = [2,3,1,1,4]
```

After reaching index 1,

we can jump as far as

```text
1 + 3 = 4
```

Now it doesn't matter **how** we reached index 1.

The only thing that matters is:

```text
What's the farthest index we can reach now?
```

Once an index becomes reachable,

keeping the maximum reachable position is always optimal.

If we can continuously extend this range until the last index,

the answer is true.

If an index lies outside this range,

no future jump can reach it.

---