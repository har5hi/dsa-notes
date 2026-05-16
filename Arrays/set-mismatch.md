code :

class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        unordered_set <int> s;
        int n = nums.size();

        int dupe = -1;
        int missing = -1;

        for(int num : nums){
            if(s.find(num) != s.end()){
                dupe = num;
            }
            s.insert(num);
        }
        for(int i = 1; i<=n; i++){
            if(s.find(i) == s.end()){
                missing = i;
            }
        }
        return{dupe,missing};
    }
};

#Time Complexity - O(n)
#Space Complexity - O(n)

mistake i made was - for(int i = 0; i<n; i++)
You are checking
0 → n-1 - BUT the problem says numbers are from: 1 → n
For: nums = [1,1] n = 2
Your loop checks:

i = 0
i = 1
0 is not in set → missing becomes 0
but 2 is never checked 

That’s why output becomes wrong.

Change:
for(int i = 0; i<n; i++)
to:
for(int i = 1; i<=n; i++)