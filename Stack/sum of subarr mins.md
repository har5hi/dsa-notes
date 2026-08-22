# LeetCode 907 - Sum of Subarray Minimums

---

# Problem Statement

Given an integer array `arr`, find the **sum of the minimum value of every subarray**.
Since the answer can be very large, return it modulo **10⁹ + 7**.

---

## Example 1

```text
Input

arr = [3,1,2,4]
```

Output

```text
17
```

---

# Brute Force Approach

Generate every subarray.
Find its minimum.
Add it to the answer.

---

## Code

```cpp
class Solution {
public:
    int sumSubarrayMins(vector<int>& arr) {

        int n = arr.size();
        long long ans = 0;
        const int MOD = 1e9+7;

        for(int i=0;i<n;i++){
            int mn = INT_MAX;
            for(int j=i;j<n;j++){
                mn=min(mn,arr[j]);
                ans=(ans+mn)%MOD;
            }
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

# Optimal Intuition

Instead of finding the minimum of every subarray,

think differently.

Ask

> **For every element, in how many subarrays is this element the minimum?**

If an element is the minimum in

```
k
```

subarrays, then its total contribution is

```text
arr[i] × k
```

Finally,

```
Sum all contributions.
```

---

# Key Observation

For every element,

find

- Previous Smaller Element (PSE)
- Next Smaller Element (NSE)

These determine how far this element can extend while remaining the minimum.

---

## Example

```text
arr

3 1 2 4
```

Consider

```text
2
```

Previous Smaller

```text
1
```

Next Smaller

```text
none
```

So

```text
Left choices = 2

Right choices = 2
```

Contribution

```text
2 × 2 × 2 = 8
```

---

# Formula

Let

```
left = distance to Previous Smaller
```

```
right = distance to Next Smaller
```

Then

```text
Contribution = arr[i] × left × right
```

---

# Previous Smaller

For duplicates,

use

```cpp
>
```

while popping.

---

# Next Smaller

For duplicates,

use

```cpp
>=
```

while popping.

This avoids double counting.

---

# Optimal Code

```cpp
class Solution {
public:

    int sumSubarrayMins(vector<int>& arr) {

        int n = arr.size();
        vector<int> prev(n), next(n);
        stack<int> st;

        // Previous Smaller

        for(int i=0;i<n;i++){
            while(!st.empty() && arr[st.top()]>arr[i])
                st.pop();

            prev[i]=st.empty()?-1:st.top();
            st.push(i);
        }

        while(!st.empty())
            st.pop();

        // Next Smaller

        for(int i=n-1;i>=0;i--){

            while(!st.empty() && arr[st.top()]>=arr[i])
                st.pop();

            next[i]=st.empty()?n:st.top();
            st.push(i);
        }

        long long ans=0;
        const int MOD=1e9+7;

        for(int i=0;i<n;i++){
            long long left=i-prev[i];
            long long right=next[i]-i;
            ans=(ans + (arr[i]*left%MOD)*right)%MOD;
        }
        return ans;
    }
};
```

---

# Why Different Comparisons?

This is the most important interview question.

For duplicate elements, we want **only one** occurrence to claim each subarray.

Therefore

### Previous Smaller

```cpp
>
```

Strictly Smaller

---

### Next Smaller

```cpp
>=
```

Smaller or Equal

This consistently breaks ties.

You can also reverse the convention:

- Previous `>=`
- Next `>`

Both are correct as long as **one side is strict and the other is non-strict**.

---

# Time Complexity

Each element is

- pushed once
- popped once

```
O(n)
```

---

# Space Complexity

Stack

```
O(n)
```

Arrays

```
O(n)
```

Total

```
O(n)
```

---

# Pattern Recognition

If the problem says

- Sum of Subarray Minimums
- Sum of Subarray Maximums
- Largest Rectangle in Histogram
- Previous Smaller
- Next Smaller

Think

> **Monotonic Stack + Contribution Technique**

---