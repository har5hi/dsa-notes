# 155. Min Stack

---

# Problem Statement

Design a stack that supports the following operations in **O(1)** time.

- `push(val)` → Push an element onto the stack.
- `pop()` → Remove the top element.
- `top()` → Return the top element.
- `getMin()` → Return the minimum element currently present in the stack.

You must implement a class `MinStack`.

---

## Example

```text
Input

["MinStack","push","push","push","getMin","pop","top","getMin"]

[[],[-2],[0],[-3],[],[],[],[]]

Output

[null,null,null,null,-3,null,0,-2]
```

### Explanation

```text
push(-2)

Stack

-2

Minimum = -2
```

```text
push(0)

Stack

0
-2

Minimum = -2
```

```text
push(-3)

Stack

-3
0
-2

Minimum = -3
```

```text
getMin()

-3
```

```text
pop()

Stack

0
-2

Minimum = -2
```

```text
top()

0
```

```text
getMin()

-2
```

---

# Intuition

A normal stack can easily return the top element in **O(1)**.

But finding the **minimum** is difficult.

Suppose

```text
Stack

5
2
8
1
```

The minimum is

```text
1
```

Now if we pop

```text
1
```

How do we know the next minimum?

We would have to scan the whole stack again.

That takes

```text
O(n)
```

which violates the problem constraints.

We need a way to **remember the minimum at every stage**.

---

# Brute Force Approach

## Idea

Store elements in a normal stack.

Whenever `getMin()` is called,

traverse the entire stack and find the smallest element.

---

## Algorithm

1. Push normally.
2. Pop normally.
3. For `getMin()`
   - Traverse entire stack.
   - Return minimum.

---

## Code

```cpp
class MinStack {
    stack<int> st;

public:

    void push(int val){
        st.push(val);
    }
    void pop(){
        st.pop();
    }
    int top(){
        return st.top();
    }
    int getMin(){

        stack<int> temp = st;
        int mini = INT_MAX;

        while(!temp.empty()){
            mini = min(mini,temp.top());
            temp.pop();
        }
        return mini;
    }
};
```

---

## Complexity

```text
Push      O(1)

Pop       O(1)

Top       O(1)

getMin    O(n)
```

Not acceptable.

---

# Optimal Approach (Two Stacks)

## Idea

Maintain

- One normal stack
- One minimum stack

The minimum stack stores the **minimum value till that point**.

Example

```text
Push 5

Stack      MinStack

5          5
```

Push 2

```text
Stack      MinStack

2          2
5          5
```

Push 8

```text
Stack      MinStack

8          2
2          2
5          5
```

Notice

Minimum remains

```text
2
```

---

# Algorithm

### Push(x)

Push into normal stack.

If MinStack is empty

push x.

Otherwise

push

```text
min(x,current minimum)
```

---

### Pop()

Pop from both stacks.

---

### Top()

Return top of normal stack.

---

### getMin()

Return top of minimum stack.

---

# Optimal Code

```cpp
class MinStack {
    stack<int> st;
    stack<int> minSt;

public:

    MinStack() {
    }

    void push(int val) {
        st.push(val);

        if(minSt.empty())
            minSt.push(val);

        else
            minSt.push(min(val,minSt.top()));
    }

    void pop() {
        st.pop();
        minSt.pop();
    }

    int top() {
        return st.top();
    }

    int getMin() {
        return minSt.top();
    }
};
```

---

# Why Does This Work?

Suppose

```text
Push

5

Current Min = 5
```

Push

```text
3

Current Min = 3
```

Push

```text
8

Current Min = 3
```

Push

```text
2

Current Min = 2
```

MinStack becomes

```text
2
3
3
5
```

Whenever we pop, both stacks pop together, so the previous minimum is automatically restored.
No searching is required.

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| Push | O(1) |
| Pop | O(1) |
| Top | O(1) |
| getMin | O(1) |

---

# Space Complexity

```text
O(n)
```

because of the extra stack.

---

# Pattern Recognition

Whenever you hear

- Current minimum
- Running minimum
- Running maximum
- Constant-time retrieval
- Design a data structure
- Maintain additional information

Think

> **Use an Auxiliary Stack (or Auxiliary Data Structure)**

This is a common interview design pattern.

---