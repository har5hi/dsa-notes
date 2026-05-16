# Set Mismatch

## Code

```cpp
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        unordered_set<int> s;
        int n = nums.size();

        int dupe = -1;
        int missing = -1;

        for(int num : nums) {
            if(s.find(num) != s.end()) {
                dupe = num;
            }
            s.insert(num);
        }

        for(int i = 1; i <= n; i++) {
            if(s.find(i) == s.end()) {
                missing = i;
            }
        }

        return {dupe, missing};
    }
};
```

## Time Complexity
- O(n)

## Space Complexity
- O(n)

## Mistake I Made
Initially I used:

```cpp
for(int i = 0; i < n; i++)
```

But the problem states numbers are from **1 → n**, not **0 → n-1**.

Example:
nums = [1,1], n = 2

Checking from 0 misses the number 2 completely.

That is why the o/p becomes wrong
Change: 
```cpp
for(int i = 0; i<n; i++) 
to: 
for(int i = 1; i<=n; i++)
```