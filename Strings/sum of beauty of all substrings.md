# LeetCode 1781 - Sum of Beauty of All Substrings

---

# Problem Statement

The **beauty** of a string is defined as:

```text
Maximum Frequency of any Character - Minimum Frequency of any Character
```

where the minimum frequency is calculated **only among the characters that appear in the substring**. Given a string `s`, return the **sum of beauty of all possible substrings**.

---

# Example 1

```text
Input:

s = "aabcb"
```

Output

```text
5
```

Explanation

All substrings are

| Substring | Frequency | Beauty |
|-----------|-----------|--------|
| a | a=1 | 0 |
| aa | a=2 | 0 |
| aab | a=2,b=1 | 1 |
| aabc | a=2,b=1,c=1 | 1 |
| aabcb | a=2,b=2,c=1 | 1 |
| a | a=1 | 0 |
| ab | a=1,b=1 | 0 |
| abc | a=1,b=1,c=1 | 0 |
| abcb | a=1,b=2,c=1 | 1 |
| b | b=1 | 0 |
| bc | b=1,c=1 | 0 |
| bcb | b=2,c=1 | 1 |
| c | c=1 | 0 |
| cb | c=1,b=1 | 0 |
| b | b=1 | 0 |

Total Beauty

```text
5
```

---

# Approaches

---

# 1. Brute Force (O(n³))

# Algorithm

1. Generate every substring.
2. Create a fresh frequency array.
3. Count frequencies.
4. Find maximum frequency.
5. Find minimum non-zero frequency.
6. Add beauty.
7. Return total beauty.

---

# Brute Force Code

```cpp
class Solution {
public:

    int beautySum(string s) {

        int n = s.length();
        int ans = 0;

        for(int i = 0; i < n; i++) {
            for(int j = i; j < n; j++) {

                vector<int> freq(26,0);

                // Count frequencies from scratch
                for(int k = i; k <= j; k++) {
                    freq[s[k]-'a']++;
                }

                int maxi = 0;
                int mini = INT_MAX;

                for(int x : freq) {
                    if(x > 0) {
                        maxi = max(maxi,x);
                        mini = min(mini,x);
                    }
                }
                ans += (maxi-mini);
            }
        }
        return ans;
    }
};
```

---

# Complexity

Generating substrings

```text
O(n²)
```

Counting frequencies

```text
O(n)
```

Finding max & min

```text
O(26)
```

Overall

```text
O(n³)
```

Space

```text
O(26) ≈ O(1)
```

---

# 2. Optimal Approach (Incremental Frequency Counting)

---

# Intuition

Instead of counting frequencies from scratch for every substring,
extend the substring one character at a time.
Update the frequency array immediately.

Example

```text
a
```

Frequency

```text
a → 1
```

Now extend

```text
aa
```

Instead of recounting,

simply do

```text
a → 2
```

Now extend

```text
aab
```

Simply update

```text
b → 1
```

We never recalculate previous frequencies.

This saves a lot of work.

---

# Algorithm

For every starting index

1. Create a frequency array.
2. Extend substring one character at a time.
3. Update frequency of newly added character.
4. Find maximum frequency.
5. Find minimum non-zero frequency.
6. Add beauty.
7. Continue expanding.

---

# Optimal Code (Fully Commented)

```cpp
class Solution {
public:

    int beautySum(string s) {

        int n = s.length();
        int ans = 0;

        // Choose every starting index
        for(int i = 0; i < n; i++) {

            // Frequency array for current starting point
            vector<int> freq(26,0);

            // Extend substring one character at a time
            for(int j = i; j < n; j++) {

                // Add current character
                freq[s[j]-'a']++;

                int maxi = 0;
                int mini = INT_MAX;

                // Find maximum and minimum frequency
                for(int k = 0; k < 26; k++) {
                    if(freq[k] > 0) {
                        maxi = max(maxi, freq[k]);
                        mini = min(mini, freq[k]);
                    }
                }
                ans += (maxi - mini);
            }
        }
        return ans;
    }
};
```

---

# Complexity

Outer loop

```text
O(n)
```

Inner loop

```text
O(n)
```

Finding max & min

```text
O(26)
```

Overall

```text
O(26 × n²)

≈

O(n²)
```

Space

```text
O(26)

≈

O(1)
```

---