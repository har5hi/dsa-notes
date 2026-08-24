# LeetCode 402 - Remove K Digits

---

# Problem Statement

Given a non-negative integer `num` represented as a string and an integer `k`, remove exactly `k` digits from the number so that the **new number is the smallest possible**.

Return the resulting number as a string.

If the result is empty, return `"0"`.

---

## Example 1

```text
Input

num = "1432219"
k = 3
```

Output

```text
"1219"
```

### Explanation

Remove

```
4

3

2
```

Smallest number becomes

```
1219
```

---

# Intuition

To get the smallest possible number, we should remove a digit if a **smaller digit appears after it**.

Example

```
54
```

Removing

```
5
```

gives

```
4
```

which is smaller than

```
5
```

Hence, whenever we see a smaller digit, remove all larger digits before it (if `k > 0`).

This is a **Greedy** decision.

---

# Why Monotonic Stack?

Maintain a stack in **increasing order**.

Whenever

```
Stack Top > Current Digit
```

remove the stack top.

Example

```
143
```

Current digit

```
3
```

Stack

```
1 4
```

Since

```
4 > 3
```

Remove

```
4
```

Now

```
1 3
```

forms a smaller number.

---

# Optimal Code

```cpp
class Solution {
public:
    string removeKdigits(string num, int k) {

        stack<char> st;

        for(char digit : num){
            while(!st.empty() && k > 0 && st.top() > digit){
                st.pop();
                k--;
            }
            st.push(digit);
        }

        // Remove remaining digits
        while(k > 0){
            st.pop();
            k--;
        }

        string ans = "";

        while(!st.empty()){
            ans += st.top();
            st.pop();
        }

        reverse(ans.begin(), ans.end());

        // Remove leading zeros
        int i = 0;

        while(i < ans.size() && ans[i] == '0')
            i++;

        ans = ans.substr(i);

        if(ans.empty())
            return "0";

        return ans;
    }
};
```

---

# Time Complexity

Every digit is

- pushed once
- popped at most once

```
O(n)
```

---

# Space Complexity

Stack

```
O(n)
```

---

# Interview Tips

Whenever the question asks

- Smallest Number
- Remove Digits
- Lexicographically Smallest
- Keep Relative Order

Think

> **Greedy + Monotonic Increasing Stack**

---