# LeetCode 42 - Trapping Rain Water

---

# Problem Statement

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water can be trapped after raining.

---

## Example

```text
Input

height = [0,1,0,2,1,0,1,3,2,1,2,1]
```

Output

```text
6
```

---

# Intuition

Water can only be trapped if there is

- a taller bar on the left
- a taller bar on the right

For every index,

```
Water = min(LeftMax, RightMax) - Height
```

Example

```text
Height

3 0 2
```

```
LeftMax = 3

RightMax = 2

Water

=min(3,2)-0

=2
```

---

# Brute Force Approach

For every index

- Find maximum on the left.
- Find maximum on the right.
- Water stored

```
min(leftMax,rightMax)-height[i]
```

---

## Code

```cpp
class Solution {
public:
    int trap(vector<int>& height) {

        int n = height.size();

        int water = 0;

        for(int i=0;i<n;i++){

            int leftMax = height[i];
            int rightMax = height[i];

            for(int j=0;j<i;j++)
                leftMax = max(leftMax,height[j]);

            for(int j=i+1;j<n;j++)
                rightMax = max(rightMax,height[j]);

            water += min(leftMax,rightMax)-height[i];
        }

        return water;
    }
};
```

### Time Complexity

```
O(n²)
```

### Space Complexity

```
O(1)
```

---

# Better Approach (Prefix & Suffix Maximum)

Instead of finding left and right maximum every time, precompute them.

---

## Idea

```
LeftMax[i] = Maximum element from 0 → i
```

```
RightMax[i] = Maximum element from i → n-1
```

Then

```
Water = min(leftMax[i],rightMax[i]) - height[i]
```

---

## Code

```cpp
class Solution {
public:
    int trap(vector<int>& height) {

        int n = height.size();
        vector<int> left(n), right(n);
        left[0] = height[0];

        for(int i=1;i<n;i++)
            left[i]=max(left[i-1],height[i]);

        right[n-1]=height[n-1];

        for(int i=n-2;i>=0;i--)
            right[i]=max(right[i+1],height[i]);

        int water=0;

        for(int i=0;i<n;i++)
            water+=min(left[i],right[i])-height[i];

        return water;
    }
};
```

### Time Complexity

```
O(n)
```

### Space Complexity

```
O(n)
```

---

# Optimal Approach (Two Pointers)

## Key Observation

At every step, the amount of water depends on the **smaller boundary**.

Suppose

```text
5      8
█      █
█      █
█  ?   █
█__█___█
```

The water level is decided by

```
min(5,8) =5
```

Not by

```
8
```

Hence, always process the side having the **smaller maximum**.

---

# Why Does This Work?

Suppose

```
LeftMax < RightMax
```

Then

```
Water = min(LeftMax,RightMax) = LeftMax
```

Since the right boundary is already taller, we don't need to know its exact value.

Left side alone determines water.

Similarly,

if

```
RightMax < LeftMax
```

process the right side.

---

# Optimal Code (Two Pointers)

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int l = 0;
        int r = height.size() - 1;
        
        int lmax = 0;
        int rmax = 0;
        
        int ans = 0;

        while(l < r){
            lmax = max(lmax, height[l]);
            rmax = max(rmax, height[r]);

            if(lmax < rmax){
                ans += lmax - height[l];
                l++;
            }
            else{
                ans += rmax - height[r];
                r--;
            }
        }
        return ans;
    }
};
```

---

# Time Complexity

Each pointer moves once.

```
O(n)
```

---

# Space Complexity

Only variables are used.

```
O(1)
```

---

# Why Not Stack?

A stack solution also exists.

It finds trapped water whenever a taller bar appears.

However,

- More difficult to understand.
- Uses O(n) extra space.

The two-pointer solution is simpler and optimal.

---

# Interview Tips

When you hear

- Rain Water
- Elevation Map
- Water Between Buildings

Think

```
Water = min(leftMax,rightMax) - height
```

Then ask

Can I compute leftMax and rightMax without arrays?

Answer:

> **Yes — Two Pointers**

---