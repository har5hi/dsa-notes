# LeetCode 45 - Jump Game II

## 📌 Problem Statement

You are given an integer array `nums`.

Each element `nums[i]` represents the **maximum jump length** from that position.

Your goal is to reach the **last index** using the **minimum number of jumps**.

It is guaranteed that you can always reach the last index.

Return the minimum number of jumps required.

---

## Example 1

```text
Input

nums = [2,3,1,1,4]

Output

2
```

### Explanation

```
Jump 1

Index 0 → Index 1
```

```
Jump 2

Index 1 → Index 4
```

Minimum jumps = **2**

---

## Example 2

```text
Input

nums = [2,3,0,1,4]

Output

2
```

---

# 💡 Intuition

Unlike **Jump Game I**, here we need the **minimum number of jumps**.

Imagine every jump gives us a **range of reachable indices**.

Example

```text
nums = [2,3,1,1,4]
```

From index 0

```text
Reachable

[1,2]
```

Now while exploring this range,

find the position that lets us reach the farthest.

Once we've finished the current range,

we **must make one jump**.

Then the next range starts.

This is exactly like a **Breadth-First Search (BFS)** on an array, where each jump represents one level.

---

# Optimal Approach (Greedy)

Maintain three variables:

```text
jumps
currentEnd
farthest
```

### Meaning

**currentEnd**

The farthest index reachable using the current number of jumps.

---

**farthest**

The farthest index we can reach while exploring the current range.

---

Whenever we reach

```text
i == currentEnd
```

it means we've explored the entire current jump range.

So,

- Make one jump.
- Update the new range.

---

# C++ Code

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {

        int jumps = 0;
        int currentEnd = 0;
        int farthest = 0;

        for (int i = 0; i < nums.size() - 1; i++) {

            farthest = max(farthest, i + nums[i]);

            if (i == currentEnd) {
                jumps++;
                currentEnd = farthest;
            }
        }
        return jumps;
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

Only three variables.

```text
O(1)
```

---

# ▶️ Dry Run

### Input

```text
nums = [2,3,1,1,4]
```

Initially

```text
jumps = 0

currentEnd = 0

farthest = 0
```

---

### Index 0

Reach

```text
0 + 2 = 2
```

```text
farthest = 2
```

Now

```text
i == currentEnd
```

Yes.

Make one jump.

```text
jumps = 1

currentEnd = 2
```

Current jump covers

```text
Indices 1 to 2
```

---

### Index 1

Reach

```text
1 + 3 = 4
```

```text
farthest = 4
```

---

### Index 2

Reach

```text
2 + 1 = 3
```

No improvement.

Now

```text
i == currentEnd
```

Yes.

Second jump.

```text
jumps = 2

currentEnd = 4
```

Last index already lies inside this range.

Answer

```text
2
```

---

# 🔍 Line-by-Line Explanation

```cpp
int jumps = 0;
```

Stores the minimum jumps taken so far.

---

```cpp
int currentEnd = 0;
```

End of the current jump range.

---

```cpp
int farthest = 0;
```

Farthest index reachable while exploring the current range.

---

```cpp
for(int i = 0; i < nums.size()-1; i++)
```

We don't process the last index because reaching it doesn't require another jump.

---

```cpp
farthest = max(farthest, i + nums[i]);
```

Update the farthest reachable position.

---

```cpp
if(i == currentEnd)
```

Finished exploring the current jump range.

Need one more jump.

---

```cpp
jumps++;
```

Increase jump count.

---

```cpp
currentEnd = farthest;
```

Start exploring the next range.

---

```cpp
return jumps;
```

Minimum jumps required.

---

# 🎯 Why Greedy Works?

Think of each jump as covering a **range** of indices.

Example

```text
nums = [2,3,1,1,4]
```

First jump reaches

```text
[1,2]
```

While scanning this range,

we calculate the farthest position we can reach.

Instead of deciding immediately where to jump,

we wait until the current range ends.

Then we make exactly **one jump** to extend our range as much as possible.

This guarantees the fewest jumps because:

- Every jump covers the maximum possible future range.
- We never make an unnecessary jump before exhausting the current range.

This is equivalent to performing **BFS level by level**, where each level represents one jump.

---

# 💼 Interview Tips

### Hint 1

If the problem asks for the **minimum number of jumps**, think about processing indices level by level.

---

### Hint 2

Don't actually perform BFS with a queue.

The array itself can represent BFS levels using:

- `currentEnd`
- `farthest`

---

### Common Mistakes

❌ Looping until `n` instead of `n-1`.

❌ Incrementing jumps at every index.

❌ Updating `currentEnd` before reaching the end of the current range.

❌ Confusing this with Jump Game I.

---

# 🧠 Pattern Recognition

This is a **Greedy + BFS Level Traversal** problem.

### Pattern

- Each jump represents one BFS level.
- Maintain the current reachable range.
- Compute the farthest next range.
- Increase jumps only when the current range is exhausted.

Similar Problems:

- LC 55 – Jump Game
- LC 134 – Gas Station
- LC 1024 – Video Stitching
- Minimum intervals to cover a range
- BFS level traversal problems

---

# ⚖️ Jump Game I vs Jump Game II

| Feature | Jump Game I | Jump Game II |
|---------|-------------|--------------|
|Goal|Can we reach the end?|Minimum jumps|
|Greedy State|Maximum reachable index|Current range + farthest next range|
|Answer|Boolean|Integer|
|Time Complexity|O(n)|O(n)|

---

# 🎯 Visual Understanding

For

```text
nums = [2,3,1,1,4]
```

```
Index

0   1   2   3   4
│
├─────────────┐
│             │
Jump 1 covers │
Indices 1-2   │
              │
      ├─────────────────────┐
      │                     │
      Jump 2 reaches Index 4│
```

Each jump expands the reachable window.

---
