# Count Number of Substrings with Exactly K Distinct Characters

---

# Problem Statement

You are given:

- A string `s`
- A positive integer `k`

Return the **number of substrings** that contain **exactly `k` distinct characters**.

---

# Understanding the Problem

Suppose

```text
s = "abc"
```

All substrings are

```text
a

ab

abc

b

bc

c
```

Now suppose

```text
k = 2
```

We need substrings having exactly **2 distinct characters**.

```text
ab ✅

bc ✅

---

# Key Observation

Finding **exactly K** directly is difficult.
Instead, think about

```text
At Most K
```

Why?

Because

```text
Exactly(K) = AtMost(K) - AtMost(K-1)
```

This is the most important observation of the problem.

---

# Why Does This Formula Work?

Suppose

```text
At Most 3
```

contains

```text
1 distinct

2 distinct

3 distinct
```

Suppose

```text
At Most 2
```

contains

```text
1 distinct

2 distinct
```

Subtract

```text
AtMost(3) - AtMost(2)
```

Only

```text
3 distinct
```

remains.

Similarly,

```text
Exactly(K) = AtMost(K) - AtMost(K-1)
```

---

# Approaches

---

# 1. Brute Force

## Algorithm

1. Generate every substring.
2. Count distinct characters.
3. If distinct count equals k,
   increment answer.
4. Return answer.

---

## Code

```cpp
class Solution{
public:

    int countSubstr(string s, int k){

        int ans = 0;

        for(int i = 0; i < s.size(); i++){
            unordered_set<char> st;

            for(int j = i; j < s.size(); j++){
                st.insert(s[j]);

                if(st.size() == k)
                    ans++;
                else if(st.size() > k)
                    break;
            }
        }
        return ans;
    }
};
```

---

## Complexity

**Time :** O(n²)

**Space :** O(k)

---

# 2. Optimal Approach (Sliding Window)

## Intuition

Instead of checking every substring, maintain a sliding window.

The window always contains

```text
At Most K Distinct Characters
```

Whenever distinct characters become

```text
>K
```

shrink the window.

At every index, count how many valid substrings end there.

---

# Helper Function

The helper returns

```text
Number of substrings

having

AT MOST K

distinct characters.
```

---

# Code (Optimal)

```cpp
class Solution {
public:

    int atMostK(string s, int k) {

        if(k < 0)
            return 0;

        unordered_map<char,int> freq;

        int left = 0;

        int ans = 0;

        for(int right = 0; right < s.size(); right++) {

            // Include current character
            freq[s[right]]++;

            // Shrink window until
            // distinct characters <= k
            while(freq.size() > k) {

                freq[s[left]]--;

                if(freq[s[left]] == 0)
                    freq.erase(s[left]);

                left++;
            }

            // Every substring ending at 'right'
            // and starting from left...right
            // is valid.
            ans += (right - left + 1);
        }
        return ans;
    }
    int countSubstr(string s, int k) {
        return atMostK(s,k) - atMostK(s,k-1);
    }
};
```

---

## Complexity

**Time :** O(n)

**Space :** O(k)

---

# Understanding Every Line

---

```cpp
unordered_map<char,int> freq;
```

Stores

```text
Character

↓

Frequency
```

Suppose window contains

```text
abac
```

Map

```text
a → 2

b → 1

c → 1
```

---

```cpp
left = 0;
```

Left boundary of window.

Initially

```text
^

abcabc
```

---

```cpp
for(right=0;right<n;right++)
```

Move right pointer.

Window grows.

Initially

```text
a
```

Then

```text
ab
```

Then

```text
abc
```

Then

```text
abca
```

---

```cpp
freq[s[right]]++;
```

Include current character.

Suppose

```text
abc
```

comes.

Map

```text
a→1

b→1

c→1
```

---

```cpp
while(freq.size()>k)
```

Suppose

```text
k=2
```

Window

```text
abc
```

Distinct

```text
3
```

Not allowed.

Shrink.

---

```cpp
freq[s[left]]--;
```

Remove leftmost character.

Suppose

```text
abc
```

Remove

```text
a
```

Map

```text
a→0

b→1

c→1
```

---

```cpp
if(freq[s[left]]==0)
```

No occurrence left.

Delete it.

Now map

```text
b→1

c→1
```

Distinct

```text
2
```

Valid.

---

```cpp
left++;
```

Window becomes

```text
bc
```

---

```cpp
ans += (right-left+1);
```

This is the most important line.

---

# Why

```cpp
right-left+1
```

?

Suppose

Window

```text
abcde
```

Current valid window

```text
cde
```

```text
left

↓

2

right

↓

4
```

Substrings ending at

```text
e
```

are

```text
e

de

cde
```

Count

```text
3
```

Formula

```text
right-left+1

=

4-2+1

=

3
```

Exactly correct.

---

# Dry Run

Input

```text
s="pqpqs"

k=2
```

Window

```text
p
```

Answer

```text
1
```

---

Window

```text
pq
```

Answer

```text
1+2=3
```

Substrings

```text
q

pq
```

---

Window

```text
pqp
```

Answer

```text
3+3=6
```

Substrings

```text
p

qp

pqp
```

---

Window

```text
pqpq
```

Answer

```text
6+4=10
```

---

Window

```text
pqpqs
```

Distinct

```text
3
```

Shrink until

```text
qs
```

Answer continues.

Finally

```text
AtMost(2)=12

AtMost(1)=5

Exactly2

=

12-5

=

7
```

---

# Why Does Sliding Window Work?

Instead of checking every substring,

we always maintain a **valid window**.

Every substring ending at `right`

and starting anywhere between

```text
left

↓

right
```

is automatically valid.

Hence

```cpp
ans += right-left+1;
```

counts all of them in O(1).

---