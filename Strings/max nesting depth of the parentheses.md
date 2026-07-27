# LeetCode 1614 - Maximum Nesting Depth of the Parentheses,

---

# Problem Statement

A string consists of:

- `'('`
- `')'`
- Digits
- Operators (`+`, `-`, `*`, `/`)
- Other characters

The **nesting depth** is the maximum number of parentheses that are open at the same time.

Return the **maximum nesting depth** of the given string.

> It is guaranteed that the parentheses are balanced.

---

# Understanding the Problem

Consider

```text
((()))
```

Let's see how many parentheses are open.

```
(

Depth = 1
```

```
((

Depth = 2
```

```
(((

Depth = 3
```

Now closing starts.

```
((()

Depth = 2
```

```
(())

Depth = 1
```

```
(()))

Depth = 0
```

The **maximum depth reached** was

```text
3
```

So answer is

```text
3
```

---

# Key Observation

Every time we encounter

```text
(
```

the nesting depth increases.

Every time we encounter

```text
)
```

the nesting depth decreases.

Therefore,

we only need two variables.

```
currentDepth
```

and

```
maximumDepth
```

---

# Approaches

---

# 1. Brute Force (Using Stack)

## Idea

A stack naturally keeps track of opened parentheses.

Every time we see

```
(
```

push it.

Every time we see

```
)
```

pop it.

The current size of the stack equals the current nesting depth.

The maximum stack size is the answer.

---

## Algorithm

1. Create an empty stack.
2. Traverse the string.
3. Push '(' into stack.
4. Update maximum stack size.
5. Pop when ')' is found.
6. Return maximum size.

---

## Code (C++)

```cpp
class Solution {
public:
    int maxDepth(string s) {

        stack<char> st;

        int ans = 0;

        for(char ch : s) {

            if(ch == '(') {

                st.push(ch);

                ans = max(ans, (int)st.size());
            }

            else if(ch == ')') {

                st.pop();
            }
        }

        return ans;
    }
};
```

---

## Complexity

**Time :** O(n)

**Space :** O(n)

---

# 2. Optimal Approach (Counter)

## Intuition

Do we actually need to store every parenthesis?

No.

We only care about

```
How many are currently open?
```

Instead of a stack,

just keep an integer.

Whenever

```
(
```

comes,

increase it.

Whenever

```
)
```

comes,

decrease it.

The largest value reached is our answer.

---

## Algorithm

1. Initialize

```text
depth = 0

maxDepth = 0
```

2. Traverse the string.

3. If

```
(
```

Increase depth.

4. Update maximum depth.

5. If

```
)
```

Decrease depth.

6. Return maximum depth.

---

# Code (Optimal)

```cpp
class Solution {
public:
    int maxDepth(string s) {

        // Stores current nesting depth
        int depth = 0;

        // Stores maximum depth encountered
        int maxDepth = 0;

        // Traverse every character
        for(char ch : s) {

            // Opening bracket increases nesting
            if(ch == '(') {

                depth++;

                // Update answer if current depth is larger
                maxDepth = max(maxDepth, depth);
            }

            // Closing bracket decreases nesting
            else if(ch == ')') {

                depth--;
            }
        }

        return maxDepth;
    }
};
```

---

## Complexity

**Time :** O(n)

**Space :** O(1)

---

# Understanding the Optimal Solution Line by Line

---

```cpp
int depth = 0;
```

This variable stores

```
How many parentheses are currently open?
```

Initially

```
0
```

because no bracket has been seen.

---

```cpp
int maxDepth = 0;
```

Stores the maximum depth found till now.

Initially

```
0
```

---

```cpp
for(char ch : s)
```

Visit every character.

Suppose

```
s

↓

"(1+(2))"
```

---

```cpp
if(ch == '(')
```

Whenever we find an opening bracket,

it means

```
One more level has started.
```

Example

Before

```
((
```

Depth

```
2
```

New bracket

```
(((
```

Depth becomes

```
3
```

So

```cpp
depth++;
```

---

```cpp
maxDepth = max(maxDepth, depth);
```

Suppose

Current

```
depth = 3
```

Previous maximum

```
2
```

Then

```cpp
max(2,3)
```

returns

```
3
```

Maximum gets updated.

If current depth is smaller,

nothing changes.

---

```cpp
else if(ch == ')')
```

Closing bracket means

```
One nesting level has ended.
```

Example

```
(((
```

Current depth

```
3
```

After

```
)
```

becomes

```
2
```

Hence

```cpp
depth--;
```

---

```cpp
return maxDepth;
```

Return the largest nesting level found.

---