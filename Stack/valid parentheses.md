# 20. Valid Parentheses

---

## Problem Statement

Given a string `s` containing just the characters:

- '('
- ')'
- '{'
- '}'
- '['
- ']'

Determine if the input string is valid.

A string is valid if:

1. Every opening bracket has a corresponding closing bracket.
2. Brackets are closed in the correct order.
3. Every closing bracket has a matching opening bracket.

---

# Intuition

The biggest observation is:

Whenever we encounter an **opening bracket**, we don't yet know when it will close.

So we temporarily **store it**.

When we encounter a **closing bracket**, it **must match the most recently opened bracket**.

This is exactly the behavior of a **Stack (LIFO)**.

Example:

```text
({[]})

Push (
Push {
Push [

Encounter ]

Top is [

Match ✓

Pop

Encounter }

Top is {

Match ✓

Pop

Encounter )

Top is (

Match ✓

Pop
```

Since the stack becomes empty, the expression is valid.

---

# Optimal Approach

## Idea

Maintain a stack.

Whenever an opening bracket comes, push it.

Whenever a closing bracket comes, check whether it matches the top.

If it doesn't, return false immediately.

Finally, the stack must be empty.

---

# Algorithm

1. Create an empty stack.
2. Traverse every character.
3. If opening bracket
   - Push it.
4. Else
   - If stack empty
     return false.
   - Compare top with current bracket.
   - If mismatch
     return false.
   - Otherwise pop.
5. After traversal,
   return true only if stack is empty.

---

# Optimal Code

```cpp
class Solution {
public:

    bool isValid(string s) {

        stack<char> st;

        for(char ch : s){
            if(ch=='(' || ch=='{' || ch=='['){
                st.push(ch);
            }

            else{
                if(st.empty())
                    return false;

                if(ch==')' && st.top()!='(')
                    return false;

                if(ch=='}' && st.top()!='{')
                    return false;

                if(ch==']' && st.top()!='[')
                    return false;

                st.pop();
            }
        }
        return st.empty();
    }
};
```

---

# Why Stack Works?

Suppose

```text
({[]})
```

Order of opening

```text
(
{
[
```

The first bracket that must close is

```
[
```

which is exactly the **last inserted** bracket.

That is

**Last In First Out**

which is Stack.

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| Traversing string | O(n) |
| Stack push | O(1) |
| Stack pop | O(1) |

Overall

```
Time : O(n)
Space : O(n)
```

---

# Interview Tips

### Tip 1

Always check

```cpp
st.empty()
```

before using

```cpp
st.top()
```

Otherwise you'll get a runtime error.

---

### Tip 2

As soon as a mismatch is found, return immediately.

Don't continue scanning.

---

### Tip 3

The final answer depends on

```cpp
st.empty()
```

because leftover opening brackets mean invalid parentheses.

---

# Pattern Recognition

Whenever you see:

- Matching symbols
- Nested structures
- Opening and closing characters
- XML/HTML tag validation
- Arithmetic expression validation
- Compiler syntax checking
- Undo operations

Think immediately:

> **Use a Stack**

This is one of the most common Stack interview patterns.

---