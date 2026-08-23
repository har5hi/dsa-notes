# LeetCode 735 - Asteroid Collision

---

# Problem Statement

We are given an array `asteroids`, where:

- Positive value (`> 0`) → asteroid moving **right**
- Negative value (`< 0`) → asteroid moving **left**

All asteroids move at the same speed.

Whenever two asteroids moving towards each other collide:

- Smaller asteroid explodes.
- If both have the same size, both explode.
- Asteroids moving in the same direction never collide.

Return the state of the asteroids after all collisions.

---

## Example 1

```text
Input

[5,10,-5]
```

Output

```text
[5,10]
```

---

# Intuition

A collision happens **only when**

```
Right-moving asteroid (+)

followed by

Left-moving asteroid (-)
```

Example

```
5 -3
```

They move towards each other.

---

These **never collide**

```
5 3
```

Same direction.

---

```
-5 -2
```

Same direction.

---

```
-5 3
```

Moving away from each other.

---

So we only need to process

```
(+ , -)
```

pairs.

---

# Why Stack?

The latest right-moving asteroid is the first one that a new left-moving asteroid can collide with.

This is exactly a **LIFO** situation.

Hence, use a stack.

---

# Optimal Code

```cpp
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& asteroids) {

        stack<int> st;

        for(int asteroid : asteroids){
            bool destroyed = false;

            while(!st.empty() && asteroid < 0 && st.top() > 0){

                if(st.top() < -asteroid){
                    st.pop();
                }

                else if(st.top() == -asteroid){
                    st.pop();
                    destroyed = true;
                    break;
                }

                else{
                    destroyed = true;
                    break;
                }
            }

            if(!destroyed)
                st.push(asteroid);
        }

        vector<int> ans(st.size());
        for(int i=st.size()-1;i>=0;i--){
            ans[i]=st.top();
            st.pop();
        }
        return ans;
    }
};
```

---

# Line-by-Line Explanation

### Stack

```cpp
stack<int> st;
```

Stores surviving asteroids.

---

### Traverse

```cpp
for(int asteroid : asteroids)
```

Process one asteroid at a time.

---

### Destroy Flag

```cpp
bool destroyed = false;
```

Keeps track of whether the current asteroid survives.

---

### Collision Condition

```cpp
while(!st.empty() && asteroid < 0 && st.top() > 0)
```

Collision only happens when

```
Stack Top -> +

Current -> -
```

---

### Current Wins

```cpp
if(st.top() < -asteroid)
```

Example

```
3

-5
```

Stack asteroid explodes.
Continue checking.

---

### Equal Size

```cpp
else if(st.top() == -asteroid)
```

Example

```
5

-5
```

Both explode.

---

### Stack Wins

```cpp
else
```

Example

```
10

-5
```

Current asteroid explodes.

---

### Push Survivor

```cpp
if(!destroyed)
    st.push(asteroid);
```

Only surviving asteroids remain in the stack.

---

### Convert Stack to Vector

Stack stores

```
Bottom

...

Top
```

Output requires original order.

So fill the answer array from the end.

---

# Time Complexity

Each asteroid is

- pushed once
- popped at most once

```
O(n)
```

---

# Space Complexity

Stack

```
O(n)
```

---