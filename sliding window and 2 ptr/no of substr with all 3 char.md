# LeetCode 1358 - Number of Substrings Containing All Three Characters

---

# Problem Statement

Given a string `s` consisting only of characters

```text
'a'

'b'

'c'
```

Return the **number of substrings** containing **at least one occurrence** of all three characters.

---

# Key Observation

Once a window contains

```text
a

b

c
```

adding more characters to the **right**

can never make it invalid.

every larger window ending further right is also valid.

This observation leads to the sliding window solution.

---

# Approaches

---

# 1. Brute Force

# Algorithm

1. Generate every substring.
2. Maintain frequency of characters.
3. Check whether

```text
a

b

c
```

are present.

4. If yes,

increment answer.

---

# Brute Force Code

```cpp
class Solution {
public:

    int numberOfSubstrings(string s) {

        int n = s.length();
        int ans = 0;

        for(int i = 0; i < n; i++) {

            vector<int> freq(3,0);

            for(int j = i; j < n; j++) {

                freq[s[j]-'a']++;

                if(freq[0] && freq[1] && freq[2])
                    ans++;
            }
        }
        return ans;
    }
};
```

---

# Complexity

Time

```text
O(n²)
```

Space

```text
O(1)
```

---

# 2. Optimal Approach (Sliding Window)

---

# Intuition

Maintain a sliding window.

Expand the window until it contains

```text
a

b

c
```

As soon as it becomes valid,

count all substrings starting from

```text
left
```

because extending the window to the right will also remain valid.

Then shrink the window from the left and continue.

---

# The Formula

Suppose

```
Length = n
```

Current window becomes valid at

```
right
```

Remaining characters

```
n-right
```

Every substring

```
left...right

left...right+1

left...right+2

...

left...n-1
```

is valid.

Therefore

```cpp
ans += (n-right);
```

This is the heart of the solution.

---

# Algorithm

1. Expand the window.
2. Store frequency of

```text
a

b

c
```

3. As soon as all three exist,

count

```cpp
n-right
```

valid substrings.

4. Remove left character.
5. Continue shrinking until window becomes invalid.
6. Expand again.

---

# Optimal Code (Fully Commented)

```cpp
class Solution {
public:

    int numberOfSubstrings(string s) {

        int n = s.length();

        vector<int> freq(3,0);

        int left = 0;
        int ans = 0;

        for(int right = 0; right < n; right++) {

            // Include current character
            freq[s[right]-'a']++;

            // While window contains a, b and c
            while(freq[0] && freq[1] && freq[2]) {

                // Count all valid substrings
                ans += (n-right);

                // Remove left character
                freq[s[left]-'a']--;
                left++;
            }
        }
        return ans;
    }
};
```

---

# Complexity

Time

```text
O(n)
```

Space

```text
O(1)
```

---

# Understanding the Optimal Solution (Sliding Window)

Before understanding the code,

remember one important observation.

We are **NOT counting one substring at a time.**

Instead,

whenever our current window contains

```text
a

b

c
```

we know that **every larger substring obtained by extending this window to the right will also be valid.**

This is the main idea behind the O(n) solution.

--- 