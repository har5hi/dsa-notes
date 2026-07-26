# LeetCode 1021 - Remove Outermost Parentheses

---

# Problem Statement

A valid parentheses string is either:

- An empty string `""`
- `"(" + A + ")"`, where `A` is a valid parentheses string
- `A + B`, where both `A` and `B` are valid parentheses strings

A **primitive** valid parentheses string is a non-empty valid string that cannot be split into two non-empty valid parentheses strings.

Given a valid parentheses string `s`, remove the **outermost parentheses** of every primitive string and return the final result.

### Example 1

```text
Input: s = "(()())(())"
Output: "()()()"
```

---

# Intuition

Every primitive starts with an **outermost '('** and ends with its **matching ')'**.

If we can identify those outer parentheses, we simply skip them while adding the remaining characters to the answer.

---

# Approaches

## 1. Brute Force (Using Stack + Primitive Extraction)

### Idea

- Traverse the string.
- Use a stack to determine when one primitive ends.
- Store each primitive separately.
- Remove its first and last character.
- Concatenate all results.

### Algorithm

1. Traverse string.
2. Push `'('` into stack.
3. Pop when `')'` is found.
4. Whenever stack becomes empty:
   - Current primitive is complete.
   - Remove first and last characters.
   - Append remaining substring.
5. Return answer.

### Code (C++)

```cpp
class Solution {
public:
    string removeOuterParentheses(string s) {
        stack<char> st;
        string ans, primitive;

        for(char c : s) {
            primitive += c;

            if(c == '(')
                st.push(c);
            else
                st.pop();

            if(st.empty()) {
                ans += primitive.substr(1, primitive.size() - 2);
                primitive = "";
            }
        }

        return ans;
    }
};
```

### Complexity

- **Time:** O(n)
- **Space:** O(n)

---

## 2. Better Approach (Count Opening & Closing Brackets)

### Idea

Instead of storing a stack, maintain two counters:

- `open`
- `close`

When `open == close`, one primitive is complete.

Again remove its first and last character.

### Algorithm

1. Traverse string.
2. Increment `open` for `'('`.
3. Increment `close` for `')'`.
4. Store current primitive.
5. Whenever `open == close`:
   - Remove outer parentheses.
   - Reset counters.

### Code

```cpp
class Solution {
public:
    string removeOuterParentheses(string s) {
        string ans, temp;
        int open = 0, close = 0;

        for(char c : s) {
            temp += c;

            if(c == '(')
                open++;
            else
                close++;

            if(open == close) {
                ans += temp.substr(1, temp.size() - 2);
                temp = "";
                open = close = 0;
            }
        }

        return ans;
    }
};
```

### Complexity

- **Time:** O(n)
- **Space:** O(n)

---

## 3. Optimal Approach (Depth Counter)

### Idea

We don't actually need to store primitives.

Maintain the current nesting depth.

### Rules

For `'('`

- If current depth > 0 → keep it.
- Increase depth.

For `')'`

- Decrease depth.
- If depth > 0 → keep it.

This automatically skips only the outermost parentheses.

---

### Dry Run

Input:

```text
(()())(())
```

| Character | Depth Before | Action | Depth After | Answer |
|-----------|--------------|--------|-------------|--------|
| ( | 0 | Skip | 1 | |
| ( | 1 | Add | 2 | ( |
| ) | 2 | Add | 1 | () |
| ( | 1 | Add | 2 | ()( |
| ) | 2 | Add | 1 | ()() |
| ) | 1 | Skip | 0 | ()() |
| ( | 0 | Skip | 1 | ()() |
| ( | 1 | Add | 2 | ()()( |
| ) | 2 | Add | 1 | ()()() |
| ) | 1 | Skip | 0 | ()()() |

Final Answer:

```text
()()()
```

---

### Algorithm

1. Initialize `depth = 0`.
2. Traverse characters.
3. If `'('`
   - If `depth > 0`, append it.
   - Increment depth.
4. Else
   - Decrement depth.
   - If `depth > 0`, append it.
5. Return answer.

---

### Code (Optimal)

```cpp
class Solution {
public:
    string removeOuterParentheses(string s) {
        string ans;
        int depth = 0;

        for(char c : s) {
            if(c == '(') {
                if(depth > 0)
                    ans += c;
                depth++;
            }
            else {
                depth--;
                if(depth > 0)
                    ans += c;
            }
        }

        return ans;
    }
};
```

---

### Complexity

- **Time:** O(n)
- **Space:** O(1) (excluding output string)

---

# Why the Optimal Approach Works

Suppose:

```text
((()))
```

Depth changes:

```text
( depth=0  -> skip
( depth=1  -> keep
( depth=2  -> keep
) depth=2→1 -> keep
) depth=1→0 -> skip
```

Only the first opening and last closing parentheses are skipped.

This happens for every primitive automatically.

---

# Interview Tips

### ✅ Observation 1

The problem is **not** asking to validate parentheses.
The string is already guaranteed to be valid.

---

### ✅ Observation 2

Think in terms of **nesting depth**, not matching pairs.
Whenever depth is zero, you're at the boundary of a primitive.

---

### ✅ Observation 3

A stack is acceptable, but unnecessary.
If you're only tracking nested levels, an integer counter is sufficient.

---

### ✅ Common Mistakes

❌ Adding `'('` before checking depth.

Correct order:

```cpp
if(depth > 0)
    ans += '(';
depth++;
```

---

❌ Checking depth after decrementing for `'('`.

Always:

```cpp
Opening:
check → increment

Closing:
decrement → check
```

---

# Key Takeaway

Whenever a problem involves **nested structures**, first ask yourself:

> **"Do I really need a stack, or is tracking the current depth enough?"**

For this problem, a simple **depth counter** replaces the stack, giving an **O(n)** time and **O(1)** extra space solution.
