# LeetCode 8 - String to Integer (atoi)

---

# Problem Statement

Implement the `myAtoi(string s)` function, which converts a string to a **32-bit signed integer**.

The algorithm should follow these steps:

1. Ignore leading whitespaces.
2. Check if the next character is `'+'` or `'-'`.
3. Read digits until a non-digit character is encountered.
4. Ignore the remaining characters.
5. Clamp the result to the 32-bit signed integer range.

The valid integer range is:

```text
[-2³¹ , 2³¹ - 1]

[-2147483648 , 2147483647]
```

---

# Understanding the Problem

The function behaves exactly like C/C++'s built-in

```cpp
atoi()
```

It should only convert the **first valid integer**.

For example

```text
"123abc"
```

Output

```text
123
```

because conversion stops at

```text
a
```

---

If the string starts with

```text
abc123
```

Output

```text
0
```

because no valid integer starts there.

---

# Approach

There is only one optimal approach.

---

# Algorithm

1. Skip leading spaces.
2. Read optional sign.
3. Read digits one by one.
4. Stop when a non-digit appears.
5. Before adding a digit, check for overflow.
6. Return the final number.

---

# Optimal Code (Fully Commented)

```cpp
class Solution {
public:
    int myAtoi(string s) {

        int i = 0;
        int n = s.size();

        // Skip all leading spaces.
        while(i < n && s[i] == ' ')
            i++;

        // Assume number is positive.
        int sign = 1;

        // Check for '+' or '-'.
        if(i < n && (s[i] == '+' || s[i] == '-')) {

            if(s[i] == '-')
                sign = -1;

            i++;
        }

        // Use long long to safely detect overflow.
        long long ans = 0;

        while(i < n && isdigit(s[i])) {

            int digit = s[i] - '0';

            ans = ans * 10 + digit;

            // Overflow check
            if(sign * ans >= INT_MAX)
                return INT_MAX;

            if(sign * ans <= INT_MIN)
                return INT_MIN;

            i++;
        }

        return sign * ans;
    }
};
```

---

# Complexity

**Time :** O(n)

**Space :** O(1)

---

# Understanding Every Line

---

```cpp
int i = 0;
```

This variable keeps track of our current position in the string.

Initially

```text
0
```

meaning

```text
First character
```

---

```cpp
int n = s.size();
```

Stores length of string.

Example

```text
"   -42"
```

Length

```text
6
```

---

```cpp
while(i<n && s[i]==' ')
```

Skip all leading spaces.

Example

```text
"    123"
```

Initially

```text
i=0
```

```
space

↓

i=1
```

```
space

↓

i=2
```

Continue until first non-space.

---

```cpp
int sign=1;
```

Assume the number is positive.

If we later find

```text
-
```

we simply change it to

```text
-1
```

---

```cpp
if(s[i]=='-' || s[i]=='+')
```

Check if there is a sign.

Example

```text
-42
```

Sign

```text
-
```

Set

```text
sign=-1
```

Move to next character.

---

```cpp
long long ans=0;
```

Why not

```cpp
int
```

?

Because

Suppose input is

```text
99999999999999999
```

Before returning

```text
INT_MAX
```

we must detect overflow.

Using

```cpp
long long
```

prevents overflow while computing.

---

```cpp
while(i<n && isdigit(s[i]))
```

Continue reading digits only.

Example

```text
123abc
```

Read

```text
1

2

3
```

Stop at

```text
a
```

---

```cpp
int digit=s[i]-'0';
```

Suppose

```text
'7'
```

ASCII

```text
55
```

Subtract

```text
'0'
```

ASCII

```text
48
```

Result

```text
7
```

Now we have the actual integer.

---

```cpp
ans=ans*10+digit;
```

This is the most important line.

Suppose

Current answer

```text
12
```

Next digit

```text
3
```

Multiply

```text
12×10

↓

120
```

Add

```text
3
```

Result

```text
123
```

Exactly how humans build numbers.

---

```cpp
if(sign*ans>=INT_MAX)
```

Suppose

```text
ans

↓

3000000000
```

Positive sign.

This exceeds

```text
2147483647
```

Return

```text
INT_MAX
```

---

```cpp
if(sign*ans<=INT_MIN)
```

Suppose

```text
-4000000000
```

Smaller than

```text
-2147483648
```

Return

```text
INT_MIN
```

---

```cpp
return sign*ans;
```

If

```text
sign=1
```

Return positive.

If

```text
sign=-1
```

Return negative.

---

# Overflow Check Explained

Suppose
Current answer

```text
214748364
```

Next digit

```text
9
```

New number

```text
2147483649
```

This is larger than

```text
2147483647
```

Therefore

```cpp
return INT_MAX;
```

Without this check,

the integer would overflow.

---

## Small improvement for interviews

The solution above is accepted, but many interviewers prefer an overflow check before performing:

```cpp 
ans = ans * 10 + digit; 
```

instead of using long long, because it works even if ans is an int. The most interview-preferred version:

```cpp 
if(ans > INT_MAX/10 || (ans == INT_MAX/10 && digit > 7)) 
```

This condition is commonly asked in FAANG interviews because it demonstrates a deeper understanding of integer overflow.

Input String

↓

① Skip Spaces

↓

② Read Sign (+ / -)

↓

③ Read Digits One by One

↓

④ Before Adding a Digit,
Check Overflow

↓

Return Answer