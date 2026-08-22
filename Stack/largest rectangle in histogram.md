# LeetCode 84 - Largest Rectangle in Histogram

---

# Problem Statement

Given an array `heights[]` representing the heights of histogram bars (each bar has width `1`), return the **area of the largest rectangle** that can be formed in the histogram.

---

## Example 1

```text
Input

heights = [2,1,5,6,2,3]
```

Output

```text
10
```

---

# Intuition

Suppose we choose one bar as the **height** of the rectangle.

How far can we expand?

- Expand left until a **smaller** bar appears.
- Expand right until a **smaller** bar appears.

So for every bar we need

- Previous Smaller Element (PSE)
- Next Smaller Element (NSE)

Then

```
Width = Next Smaller - Previous Smaller - 1
```

Finally,

```
Area = Height × Width
```

Take the maximum area.

---

# Brute Force Approach

For every bar

- Expand left until a smaller element.
- Expand right until a smaller element.
- Compute area.

---

## Code

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {

        int n = heights.size();
        int ans = 0;

        for(int i=0;i<n;i++){

            int left=i;
            int right=i;

            while(left>0 && heights[left-1]>=heights[i])
                left--;

            while(right<n-1 && heights[right+1]>=heights[i])
                right++;

            ans=max(ans,(right-left+1)*heights[i]);
        }
        return ans;
    }
};
```

---

### Time Complexity

```
O(n²)
```

---

### Space Complexity

```
O(1)
```

---

# Better Approach (PSE + NSE)

## Observation

Instead of expanding every time,

compute

- Previous Smaller
- Next Smaller

using a Monotonic Stack.

---

# Previous Smaller Element

Example

```
2 1 5 6 2 3

PSE

-1 -1 1 2 1 4
```

---

# Next Smaller Element

```
2 1 5 6 2 3

NSE

1 6 4 4 6 6
```

If no smaller element exists,

```
PSE = -1

NSE = n
```

---

# Code

```cpp
class Solution {
public:

    vector<int> previousSmaller(vector<int>& h){

        int n=h.size();
        vector<int> ans(n);
        stack<int> st;

        for(int i=0;i<n;i++){

            while(!st.empty() && h[st.top()]>=h[i])
                st.pop();

            if(st.empty())
                ans[i]=-1;
            else
                ans[i]=st.top();

            st.push(i);
        }

        return ans;
    }

    vector<int> nextSmaller(vector<int>& h){

        int n=h.size();
        vector<int> ans(n);
        stack<int> st;

        for(int i=n-1;i>=0;i--){

            while(!st.empty() && h[st.top()]>=h[i])
                st.pop();

            if(st.empty())
                ans[i]=n;
            else
                ans[i]=st.top();

            st.push(i);
        }
        return ans;
    }

    int largestRectangleArea(vector<int>& heights) {

        vector<int> left=previousSmaller(heights);
        vector<int> right=nextSmaller(heights);

        int ans=0;

        for(int i=0;i<heights.size();i++){
            int width=right[i]-left[i]-1;
            ans=max(ans,width*heights[i]);
        }
        return ans;
    }
};
```

---

### Time Complexity

```
O(n)
```

---

### Space Complexity

```
O(n)
```

---

# Optimal Approach (Single Monotonic Stack)

Instead of storing

- Previous Smaller
- Next Smaller

we can calculate the area **while popping**.

---

# Key Observation

Whenever a smaller element appears,

the current bar becomes the **Next Smaller Element** for all taller bars in the stack.

That means their rectangle ends here.

So calculate their area immediately.

---

---

# Dry Run

```
2 1 5 6 2 3
```

---

### Push

```
2
```

Stack

```
0
```

---

### Current

```
1
```

Smaller than

```
2
```

Pop

```
2
```

```
height=2

left=-1

right=1

width=1

area=2
```

Push

```
1
```

---

### Continue

Eventually

When

```
2
```

appears,

```
6

5
```

are popped

For

```
5
```

```
left=1

right=4

width=2

area=10
```

Maximum becomes

```
10
```

---

# Optimal Code (Single Stack)

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {

        int n = heights.size();
        stack<int> st;
        int ans = 0;

        for(int i=0;i<=n;i++){

            while(!st.empty() &&
                 (i==n || heights[st.top()]>=heights[i])){

                int height = heights[st.top()];
                st.pop();

                int right = i;

                int left;

                if(st.empty())
                    left=-1;
                else
                    left=st.top();

                int width = right-left-1;

                ans=max(ans,height*width);
            }

            st.push(i);
        }

        return ans;
    }
};
```

---

# Line-by-Line Explanation

---

```cpp
while(!st.empty() &&
(i==n || heights[st.top()]>=heights[i]))
```

Current bar is smaller,

so rectangles using taller bars must end here.

---

```cpp
height = heights[st.top()]
```

Rectangle height.

---

```cpp
right=i;
```

Current index is the first smaller bar on the right.

---

```cpp
left=st.empty()?-1:st.top();
```

After popping,

the new top is the previous smaller element.

---

```cpp
width

=

right-left-1
```

Maximum width for this rectangle.

---

```cpp
area=height×width
```

Update maximum area.

---

```cpp
st.push(i);
```

Current bar may become the previous smaller element for future bars.

---

# Why Do We Use `>=` Instead of `>`?

Consider

```text
2 2 2
```

If we use

```cpp
>
```

equal heights stay in the stack,

leading to incorrect widths.

Using

```cpp
>=
```

ensures equal-height bars are merged into one maximal rectangle.

---

# Time Complexity

Each bar is

- pushed once
- popped once

Total operations

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

# Interview Tips

Whenever you hear

- Largest Rectangle
- Previous Smaller
- Next Smaller
- Histogram

Think immediately

> **Monotonic Increasing Stack**

---

# Final Takeaway

- Every bar is treated as the **minimum height** of a rectangle.
- The rectangle expands until the **previous smaller** and **next smaller** bars.
- Width is

```
NSE - PSE - 1
```

- The optimal solution uses a **Monotonic Increasing Stack**.
- Each index is pushed and popped only once, giving **O(n)** time complexity.
- **Rule to Remember:**
  > **Largest Rectangle in Histogram = Previous Smaller + Next Smaller + Monotonic Increasing Stack**