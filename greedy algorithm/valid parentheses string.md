# LeetCode 678 - Valid Parenthesis String

## 📌 Problem Statement

You are given a string `s` containing only:

- `'('`
- `')'`
- `'*'`

The `'*'` character can represent:

- `'('`
- `')'`
- an empty string `""`

Return **`true`** if the string can be made into a valid parentheses string, otherwise return **`false`**.

---

### Example 1

```text
Input:
s = "()"

Output:
true
```

---

### Example 2

```text
Input:
s = "(*)"

Output:
true
```

Explanation:

```
'*' acts as an empty string.

Result:

()
```

---

### Example 3

```text
Input:
s = "(*))"

Output:
true
```

Explanation:

```
'*' acts as '('

Result:

(())
```

---

# 💡 Intuition

The difficult part is the `'*'` character.

Every `'*'` has **three possibilities**:

```
'('
')'
''
```

Trying every possibility leads to exponential time.

Instead, observe that we don't need to know **exactly** what each `'*'` becomes.

We only need to know:

- the **minimum** possible number of unmatched `'('`
- the **maximum** possible number of unmatched `'('`

These two values are enough to determine whether the string can still become valid.

---

# Optimal Approach (Greedy)

Maintain two counters:

```
low
high
```

Where:

```
low  = minimum possible unmatched '('
high = maximum possible unmatched '('
```

Initially

```
low = 0
high = 0
```

---

### Case 1

Character is `'('`

It definitely increases unmatched openings.

```
low++
high++
```

---

### Case 2

Character is `')'`

It closes one opening.

```
low--
high--
```

---

### Case 3

Character is `'*'`

It can become:

```
(
)
or empty
```

Best case

```
Acts as ')'

low--
```

Worst case

```
Acts as '('

high++
```

So

```
low--
high++
```

---

If

```
high < 0
```

There are too many closing brackets.

Return

```
false
```

---

Also,

```
low
```

can never be negative.

If

```
low < 0
```

set

```
low = 0
```

because unmatched openings cannot be less than zero.

---

At the end,

```
low == 0
```

means a valid assignment exists.

---

# C++ Code

```cpp
class Solution {
public:
    bool checkValidString(string s) {

        int low = 0;
        int high = 0;

        for (char ch : s) {
            if (ch == '(') {
                low++;
                high++;
            }
            else if (ch == ')') {
                low--;
                high--;
            }
            else {
                low--;
                high++;
            }
            if (high < 0)
                return false;
            if (low < 0)
                low = 0;
        }
        return low == 0;
    }
};
```

---

# ⏱ Time Complexity

Single traversal.

```
O(n)
```

---

# 📦 Space Complexity

Only two variables.

```
O(1)
```

---

# 🎯 Why Greedy Works?

Instead of deciding exactly what every `'*'` becomes, we maintain a **range** of possible unmatched `'('` counts.

Suppose

```
low = 2
high = 5
```

This means after processing the current prefix, the number of unmatched `'('` could be **anywhere between 2 and 5**, depending on how we interpret previous `'*'` characters.

As we process more characters:

- `high` tracks the most optimistic scenario (treat `'*'` as `'('`).
- `low` tracks the most conservative scenario (treat `'*'` as `')'` or empty).

If `high` ever becomes negative, even the best interpretation cannot avoid extra `')'`, so the string is invalid.

If we finish with `low == 0`, at least one valid interpretation exists.

Thus, greedily maintaining the range is sufficient.

---

# ⚖️ Stack vs Greedy

| Approach | Time | Space | Recommended |
|----------|------|-------|-------------|
| Backtracking | O(3ⁿ) | O(n) | ❌ No |
| DP | O(n²) | O(n²) | ⚠️ Possible |
| Stack | O(n) | O(n) | ✅ Works |
| Greedy (`low/high`) | O(n) | O(1) | ⭐ Best |

---