# Contains Duplicate

## Intuition

- We need to check if any number appears more than once.
- Use a hash set (unordered_set) to store elements we have already seen.

If the current element is already in the set → duplicate found → return true

Otherwise insert it into the set

## Important Concept
```cpp unordered_set ```
- Stores unique elements
- Average lookup time = O(1)
- Very useful for:

duplicate checking

fast searching

removing repeated values

```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> s;

        for(int i = 0; i < nums.size(); i++) {
            if(s.find(nums[i]) != s.end()){
                return true;
            }
            s.insert(nums[i]);
        }
        return false;
    }
};
```

## Time Complexity
- Average Case:

find() → O(1)

insert() → O(1)

Loop runs n times: O(n)

## Space Complexity
- In worst case, all elements are unique and stored in set: O(n)