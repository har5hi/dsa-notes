# LeetCode 455 - Assign Cookies

## 📌 Problem Statement

Assume you are an awesome parent and want to give your children cookies.

- Each child has a **greed factor** `g[i]`, which is the minimum cookie size needed to satisfy that child.
- Each cookie has a size `s[j]`.
- A child can receive **only one cookie**.
- A cookie can be assigned to **only one child**.

Return the **maximum number of children that can be satisfied**.

### Example 1

```text
Input:
g = [1,2,3]
s = [1,1]

Output:
1
```

Explanation:
- Child with greed 1 gets cookie of size 1.
- Other child cannot be satisfied.

---

### Example 2

```text
Input:
g = [1,2]
s = [1,2,3]

Output:
2
```

---

# 💡 Intuition

We want to satisfy as many children as possible.

Suppose we give a **large cookie** to a child with **small greed**.

That large cookie could have been used for a greedier child, which may reduce the total number of satisfied children.

So,

- Give the **smallest available cookie** to the **least greedy child**.
- If it satisfies the child, move to the next child.
- Otherwise, try a larger cookie.

This naturally leads to a **Greedy Algorithm**.

---

# Brute Force Approach

For every child,

- Search every unused cookie.
- Assign the smallest cookie that satisfies the child.

### Algorithm

1. Mark every cookie as unused.
2. For every child:
   - Search all cookies.
   - Pick the smallest suitable unused cookie.
3. Count satisfied children.

### Time Complexity

```
O(n × m)
```

### Space Complexity

```
O(m)
```

Not efficient for large inputs.

---

# Optimal Approach (Greedy + Sorting)

### Key Idea

Sort both arrays.

```
Children : 1 2 3
Cookies  : 1 1 2 3
```

Start from the smallest greed and smallest cookie.

If cookie >= greed

```
Assign it.
```

Otherwise

```
Try a bigger cookie.
```

This guarantees maximum assignments.

---

# ✅ C++ Code

```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {

        sort(g.begin(), g.end());
        sort(s.begin(), s.end());

        int child = 0;
        int cookie = 0;

        while (child < g.size() && cookie < s.size()) {

            if (s[cookie] >= g[child]) {
                child++;
                cookie++;
            }
            else {
                cookie++;
            }
        }
        return child;
    }
};
```

---

# ⏱ Time Complexity

Sorting:

```
O(n log n + m log m)
```

Two-pointer traversal:

```
O(n + m)
```

Overall:

```
O(n log n + m log m)
```

---

# 📦 Space Complexity

```
O(1)
```

---

# 💼 Interview Tips

### Hint 1

Whenever the question asks for:

- Maximum assignments
- Pairing two arrays
- One-to-one matching

Think about **sorting + two pointers**.

---

### Hint 2

Ask yourself:

> Can I make a locally optimal choice that leads to a globally optimal answer?

If yes, Greedy is often applicable.

---

# 🧠 Pattern Recognition

This problem is a classic **Greedy Matching** problem.

### Pattern

- Sort both arrays
- Match smallest with smallest possible
- Use two pointers
- Never reconsider previous assignments