# LeetCode 5 - Longest Palindromic Substring

---

# Problem Statement

Given a string `s`, return the **longest palindromic substring** in `s`.

A **palindrome** is a string that reads the same forward and backward.

---

# Understanding the Problem

Suppose

```text
s = "babad"
```

Possible substrings

```text
b

ba

bab ✅

baba

babad

a

ab

aba ✅

abad

...
```

Longest palindrome

```text
bab
```

or

```text
aba
```

Length

```text
3
```

We must return the **substring**, not its length.

---

# Approaches

---

# 1. Brute Force (O(n³))

## Intuition

Generate every possible substring.
For every substring, check whether it is a palindrome.
Keep track of the longest palindrome.

---

## Algorithm

1. Generate every substring.
2. Check if the substring is a palindrome.
3. If yes,
   compare its length with the current answer.
4. Return the longest substring.

---

# Brute Force Code

```cpp
class Solution {
public:

    bool isPalindrome(string str) {

        int left = 0;
        int right = str.length() - 1;

        while(left < right) {

            if(str[left] != str[right])
                return false;

            left++;
            right--;
        }
        return true;
    }

    string longestPalindrome(string s) {

        string ans = "";

        for(int i = 0; i < s.length(); i++) {
            for(int j = i; j < s.length(); j++) {

                string sub = s.substr(i, j - i + 1);

                if(isPalindrome(sub)) {
                    if(sub.length() > ans.length())
                        ans = sub;
                }
            }
        }
        return ans;
    }
};
```

---

## Complexity

Generating all substrings

```text
O(n²)
```

Checking palindrome

```text
O(n)
```

Total

```text
O(n³)
```

Space

```text
O(1)
```

(ignoring substring creation)

---

# 2. Better Approach (Expand Around Center)

⭐ This is the **most preferred interview solution.**

---

# Intuition

Instead of checking every substring,
start from the **middle** of a palindrome and expand outward.

Example

```text
aba
```

Center

```text
b
```

Expand

```text
a b a
```

Palindrome found.

---

Another example

```text
racecar
```

Center

```text
e
```

Expand

```text
cec

↓

aceca

↓

racecar
```

Every palindrome grows from its center.

---

## Odd Length

Example

```text
aba
```

Center

```text
b
```

```
a b a
  ^
```

Only **one center**.

---

## Even Length

Example

```text
abba
```

There is **no single middle character.**

The center lies **between two characters.**

```
a b | b a
    ^
```

Therefore,

for every index,

we must check

```text
Odd Length

(center = i)
```

and

```text
Even Length

(center = i , i+1)
```

---

# Why 2n−1 Centers?

Suppose

```text
abc
```

Characters

```text
a b c
```

Odd centers

```text
a

b

c
```

Total

```text
3
```

Even centers

```text
a|b

b|c
```

Total

```text
2
```

Overall

```text
3+2

=

5

=

2n−1
```

---

Another example

```text
abcd
```

Odd

```text
a

b

c

d
```

4

Even

```text
a|b

b|c

c|d
```

3

Total

```text
7

=

2×4−1
```

---

# Algorithm

For every index

1. Expand considering it as an odd center.
2. Expand considering it as an even center.
3. Update longest palindrome.
4. Return answer.

---

# Optimal Interview Code

```cpp
class Solution {
public:

    string longestPalindrome(string s) {

        int start = 0;
        int maxLen = 1;
        int n = s.length();

        for(int i = 0; i < n; i++) {

            // ---------- Odd Length ----------
            int left = i;
            int right = i;

            while(left >= 0 &&
                  right < n &&
                  s[left] == s[right]) {

                if(right - left + 1 > maxLen) {

                    maxLen = right - left + 1;
                    start = left;
                }

                left--;
                right++;
            }

            // ---------- Even Length ----------
            left = i;
            right = i + 1;

            while(left >= 0 &&
                  right < n &&
                  s[left] == s[right]) {

                if(right - left + 1 > maxLen) {

                    maxLen = right - left + 1;
                    start = left;
                }
                left--;
                right++;
            }
        }
        return s.substr(start, maxLen);
    }
};
```

---

## Complexity

Time

```text
O(n²)
```

Space

```text
O(1)
```

---

# Why is this better?

Instead of checking

```text
Every substring
```

we only try to **expand around possible centers**.

Each expansion immediately stops when characters don't match.

This avoids generating unnecessary substrings and is much faster in practice.

---