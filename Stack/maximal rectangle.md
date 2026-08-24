# LeetCode 85 - Maximal Rectangle

---

# Problem Statement

Given a binary matrix filled with `'0'`s and `'1'`s, return the **area of the largest rectangle containing only `1`s**.

---

## Example 1

```text
Input

matrix =
[
["1","0","1","0","0"],
["1","0","1","1","1"],
["1","1","1","1","1"],
["1","0","0","1","0"]
]
```

Output

```text
6
```

---

# Brute Force Approach

For every cell,

try to expand

- right
- down

while all elements remain `1`.

Compute every possible rectangle.

---

### Time Complexity

```
O((m × n)²)
```

Very slow.

---

# Optimal Intuition

This problem is actually

> **LC 84 (Largest Rectangle in Histogram) repeated for every row.**

This is the most important observation.

---

Suppose

```
Matrix

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```

Instead of looking at the matrix, build histogram heights.

---

# Why Histogram Works?

Every column stores

```
Continuous number of 1's above it.
```

Example

```
1
1
1
```

Height becomes

```
3
```

So every row becomes the base of a histogram.

---

# Complete Optimal Code

```cpp
class Solution {
public:

    int largestRectangleArea(vector<int>& heights){

        stack<int> st;
        int n = heights.size();
        int maxArea = 0;

        for(int i = 0; i <= n; i++){

            int currHeight = (i == n ? 0 : heights[i]);

            while(!st.empty() && currHeight < heights[st.top()]){

                int height = heights[st.top()];
                st.pop();
                int width;

                if(st.empty())
                    width = i;
                else
                    width = i - st.top() - 1;

                maxArea = max(maxArea, height * width);
            }
            st.push(i);
        }
        return maxArea;
    }

    int maximalRectangle(vector<vector<char>>& matrix) {

        if(matrix.empty())
            return 0;

        int rows = matrix.size();
        int cols = matrix[0].size();
        vector<int> height(cols,0);

        int ans = 0;

        for(int i = 0; i < rows; i++){
            for(int j = 0; j < cols; j++){

                if(matrix[i][j] == '1')
                    height[j]++;
                else
                    height[j] = 0;
            }
            ans = max(ans, largestRectangleArea(height));
        }
        return ans;
    }
};
```

---

# Line-by-Line Explanation

### Height Array

```cpp
vector<int> height(cols,0);
```

Stores histogram heights.

---

### Update Heights

```cpp
if(matrix[i][j]=='1')
    height[j]++;
```

Increase consecutive height.

---

```cpp
else
    height[j]=0;
```

Reset because rectangle breaks.

---

### Histogram

```cpp
largestRectangleArea(height)
```

Treat current row as histogram.

---

### Update Answer

```cpp
ans=max(ans, largestRectangleArea(height));
```

Largest rectangle may end at this row.

---

# Why Can We Solve Every Row Independently?

Suppose

```
1
1
1
```

Current row is the bottom.
Height already stores

```
3
```

LC 84 automatically finds
the widest rectangle using these heights.
No need to inspect previous rows again.

---

# Time Complexity

Suppose

```
Rows = m

Columns = n
```

Updating heights

```
O(mn)
```

Histogram for every row

```
m × O(n)
```

Total

```
O(mn)
```

---

# Space Complexity

Height Array

```
O(n)
```

Stack

```
O(n)
```

Total

```
O(n)
```

---

# Interview Tips

The interviewer expects this observation:

> Every row can be treated as the base of a histogram.

Once you recognize that, the problem immediately becomes

```
Repeated LC 84
```

---

# Pattern Recognition

This problem is built directly on

- ⭐ LC 84 Largest Rectangle in Histogram
- LC 85 Maximal Rectangle