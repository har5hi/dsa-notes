# LeetCode 496 - Next Greater Element I

---

# Problem Statement

You are given two **0-indexed arrays** `nums1` and `nums2`, where `nums1` is a subset of `nums2`.
For each element in `nums1`, find the **first greater element** to its right in `nums2`.
If no greater element exists, return `-1` for that element.

---

## Example 1

```text
Input:
nums1 = [4,1,2]
nums2 = [1,3,4,2]

Output:
[-1,3,-1]
```

### Explanation

- 4 → No greater element on the right → -1
- 1 → Next greater is 3
- 2 → No greater element → -1

---

# Intuition

For every element, we need the **first greater element on its right**.
A brute force solution checks every element to the right, but this repeats work.
Instead, use a **Monotonic Decreasing Stack**.

---

# Brute Force Approach

For every element in `nums1`

- Find its position in `nums2`
- Scan towards the right
- Return the first greater element

---

## Brute Force Code

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {

        vector<int> ans;

        for(int x : nums1){
            int idx = -1;
            for(int i=0;i<nums2.size();i++){
                if(nums2[i]==x){
                    idx=i;
                    break;
                }
            }

            int nge = -1;

            for(int i=idx+1;i<nums2.size();i++){
                if(nums2[i]>x){
                    nge=nums2[i];
                    break;
                }
            }
            ans.push_back(nge);
        }
        return ans;
    }
};
```

### Time Complexity

```
O(n × m)
```

### Space Complexity

```
O(1)
```

---

# Optimal Approach (Monotonic Stack)

## Observation

For every element in `nums2`, we only need to know its next greater element once.

After computing this mapping, answering for `nums1` becomes O(1).

---

# Why Traverse from Right?

Suppose

```
nums2 = [1,3,4,2]
```

Process from right

```
2
4
3
1
```

When processing an element, all possible candidates on the right are already processed.
So the stack already contains useful information.

---

# Why Pop Smaller Elements?

Suppose stack

```
Top

2
3
8
```

Current element

```
5
```

Both

```
2
3
```

are useless.

Why?

Because

```
5 > 2
5 > 3
```

If someone on the left wants a greater element, 5 is closer than 2 or 3.

Hence

```
2
3
```

can never become someone's next greater element.
So remove them.

---

# Monotonic Stack Property

The stack always remains

```
Increasing from Top to Bottom

Top

4
7
9
```

or equivalently

```
Decreasing from Bottom to Top
```

---

# Optimal Code (C++)

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {

        stack<int> st;
        unordered_map<int,int> mp;

        for(int i=nums2.size()-1;i>=0;i--){
            while(!st.empty() && st.top()<=nums2[i]){
                st.pop();
            }
            if(st.empty())
                mp[nums2[i]]=-1;
            else
                mp[nums2[i]]=st.top();
            st.push(nums2[i]);
        }

        vector<int> ans;
        for(int x:nums1){
            ans.push_back(mp[x]);
        }
        return ans;
    }
};
```

---

# Time Complexity

### Building map

```
O(n)
```

Each element is pushed once and popped once.

---

### Answering queries

```
O(m)
```

---

### Total

```
O(n + m)
```

where

- n = nums2.size()
- m = nums1.size()

---

# Space Complexity

Stack

```
O(n)
```

Hash Map

```
O(n)
```

Total

```
O(n)
```

---

# Why This is a Monotonic Stack Problem

Look for these clues:

- Next Greater Element
- Previous Greater Element
- Next Smaller Element
- Previous Smaller Element
- Nearest Greater/Smaller
- First greater/smaller on left/right

Whenever you see one of these, think:

> **Monotonic Stack**

---