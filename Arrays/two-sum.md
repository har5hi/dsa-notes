# Two Sum

## Intuition

- For every number, we check: “What number do I need to reach the target?”

That required number is called the complement.

- Formula: complement=target−current number

- Use a hashmap (unordered_map) to store: number -> index
- If complement already exists in map → answer found
- Otherwise store current number and continue

## Important Concept

HashMap (unordered_map)

Stores: value -> index

Example: mp[2] = 0;

means:

- number 2
- found at index 0

Hashmaps give:

- fast search → O(1)
- fast insertion → O(1)

``` cpp
class Solution { 
    public: 
       vector<int> twoSum(vector<int>& nums, int target) { 
        unordered_map<int, int> mp; 
        
        for(int i = 0; i < nums.size(); i++){ 
            int complement = target - nums[i]; 
            
            if(mp.find(complement) != mp.end()){
                 return{ 
                    mp[complement], i 
                }; 
            } 
            mp[nums[i]] = i; 
            } 
        return {}; 
    }
};
```
## Time Complexity

- Each element is processed once.

Hashmap lookup → O(1)

Insertion → O(1)

Total: O(n)

## Space Complexity

In worst case, hashmap stores all elements.

O(n)