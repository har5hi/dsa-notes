# Daily - Temperature

## intuition 

This is a classic monotonic stack problem

We need to find, for every day, how many days later a warmer temperature occurs.

we use a monotonic decreasing stack, the stack stores indices of temperatures.

We traverse from right to left because:
- the future temperatures are already processed
- the stack helps us quickly find the next warmer temperature

For each temperature:
- Remove all smaller or equal temperatures from stack
- The top of stack becomes the next warmer day
- Store the difference of indices
- Push current index into stack

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temp) {
        int n = temp.size();

        vector<int> ans(n, 0);   // initialize all with 0
        stack<int> st;           // stores indices

        for(int i = n - 1; i >= 0; i--) {
            // remove smaller or equal temperatures
            while(!st.empty() && temp[i] >= temp[st.top()]) {
                st.pop();
            }
            // if stack not empty, warmer day exists
            if(!st.empty()) {
                ans[i] = st.top() - i;
            }
            // push current index
            st.push(i);
        }
        return ans;
    }
};
```
## What Was Wrong & What Was Corrected
## Your Code	Problem 	Correction
- for(int i = n; i >= 0; i++)	Index out of bounds because valid indices are 0 to n-1	for(int i = n-1; i >= 0; i--)
- temp[i] > st.top()	Stack should store indices, not temperatures	temp[i] >= temp[st.top()]
- st.top[i]	Wrong syntax, top() is a function	st.top()
- int ans = ... inside loop	Created a new local variable instead of updating vector	ans[i] = ...
- vector<int> ans;	Vector size not initialized	vector<int> ans(n, 0)
- Never pushed elements into stack	Stack remained empty Added st.push(i)
- temp[i] - st.top[i]	Logic incorrect — we need distance between days, not temperature difference	st.top() - i

## Time Complexity

Each element is: pushed once, popped at most once
So total operations = O(n)

Time Complexity: O(n)

## Space Complexity

Stack can store at most n indices.

Space Complexity: O(n)