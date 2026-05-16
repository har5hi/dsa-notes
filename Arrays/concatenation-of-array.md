# Concatenation of Array

## Intuition

Just take every element from nums and put it twice into a new array.

```cpp
class Solution {
public:
    vector<int> getConcatenation(vector<int>& nums) {
        vector<int> ans = nums;

        for(int i = 0; i < nums.size(); i++) {
            ans.push_back(nums[i]);
        }

        return ans;
    }
};
```

## Time Complexity

Step 1
```cpp
vector<int> ans = nums;
```

Copying all elements takes: O(n)

Step 2

Loop runs n times:

```cpp
for(int i = 0; i < nums.size(); i++)
```

Each push_back() is approximately: O(1)

So total: O(n)+O(n)=O(n)

## Space Complexity

You create a new vector: ans

which stores 2n elements.

So: O(n)

## Important Concept

When we analyze space complexity:

input array nums does NOT count
only extra memory we create counts

So:

ans

is the extra memory.

That’s why space complexity is: O(n)