# LeetCode 503 - Next Greater Element II

---

# Problem Statement

Given a **circular integer array** `nums`, return the **Next Greater Number** for every element.

The next greater number of a number `x` is the **first greater number** to its traversing-order next in the array.

Since the array is **circular**, after the last element we continue from the beginning.

If no greater element exists, return `-1`.

---

## Example 1

```text
Input:
nums = [1,2,1]

Output:
[2,-1,2]
```

--- 

## Example 2

```text
Input:
nums = [1,2,3,4,3]

Output:
[2,3,4,-1,4]
```

---

# Intuition

Suppose

```
nums

1 2 1
```

For the last element

```
1
```

its next greater element is actually

```
2
```

which lies at the beginning.

So a single traversal is not enough.

Instead of actually duplicating the array

```
1 2 1 1 2 1
```

we simply traverse

```
2 × n
```

times using

```
index = i % n
```

This is the most common trick for circular arrays.

---

# Key Idea

Pretend the array is

```
nums + nums
```

Example

```
Original

1 2 1

Imagine

1 2 1 1 2 1
```

Instead of creating this array,

use

```cpp
nums[i % n]
```

---

# Why Traverse 2*n Times?

Because every element should get a chance to look beyond the end of the array.

The second traversal provides those wrapped-around elements.

---

# Why Do We Update Answer Only When i < n?

The first

```
n
```

iterations are only used to fill the stack.

The second half corresponds to the original array indices.

If we updated answers during both traversals,

we would overwrite correct answers.

---

# Optimal Code (C++)

```cpp
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {

        int n = nums.size();
        vector<int> ans(n, -1);
        stack<int> st;

        for(int i = 2 * n - 1; i >= 0; i--){
            while(!st.empty() && st.top() <= nums[i % n]){
                st.pop();
            }
            if(i < n){
                if(!st.empty())
                    ans[i] = st.top();
            }
            st.push(nums[i % n]);
        }
        return ans;
    }
};
```

---

# Time Complexity

Although the loop runs

```
2n
```

times,

every element is

- pushed once
- popped once

Therefore

```
O(2n)

= O(n)
```

---

# Space Complexity

Stack

```
O(n)
```

Answer array

```
O(n)
```

Total

```
O(n)
```

--- 