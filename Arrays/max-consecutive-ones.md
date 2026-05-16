# Max Consecutive Ones

## Intuition

We keep:

count → current streak of 1s

maxCount → maximum streak seen so far

Rules:
if element is 1 → increase count
if element is 0 → reset count = 0

At every step:
maxCount = max(maxCount, count)

code: 
```cpp
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {

        int count = 0;
        int maxCount = 0;

        for(int i = 0; i < nums.size(); i++) {

            if(nums[i] == 1) {
                count++;
                maxCount = max(maxCount, count);
            }
            else {
                count = 0;
            }
        }

        return maxCount;
    }
};
```
## Time Complexity
- O(n)

## Space Complexity
- O(1)