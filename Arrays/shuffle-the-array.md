# Shuffle the Array

## Intuition

Take:
- one element from first half
- one from second half
- keep alternating

```cpp
class Solution {
public:
    vector<int> shuffle(vector<int>& nums, int n) {

        vector<int> ans;

        for(int i = 0; i < n; i++) {

            ans.push_back(nums[i]);     
            ans.push_back(nums[i + n]); 

        }

        return ans;
    }
};
```
## Time Complexity
- O(n)
## Space Complexity
- O(n)