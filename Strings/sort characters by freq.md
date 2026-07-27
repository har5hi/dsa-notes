# LeetCode 451 - Sort Characters By Frequency

---

# Problem Statement

Given a string `s`, sort it in **decreasing order based on the frequency** of its characters.

The frequency of a character is the number of times it appears in the string.

Return the sorted string. If multiple characters have the same frequency, any order among them is acceptable.

### Example 1

```text
Input: s = "tree"

Output: "eert"
```

or

```text
"eetr"
```

Both are correct because `'e'` appears twice.

---

# Intuition

The problem asks us to arrange characters according to **how many times they occur**.

Instead of sorting the string directly,

1. Count the frequency of every character.
2. Sort the characters based on frequency.
3. Build the answer.

---

# Approaches

## 1. Brute Force (Repeated Counting)

### Idea

For every character:

- Count its frequency by scanning the whole string.
- Select the character with the highest remaining frequency.
- Append it to the answer.

This repeats many unnecessary scans.

---

### Algorithm

1. Traverse every character.
2. Count its occurrences.
3. Find the maximum frequency character.
4. Append it.
5. Repeat until all characters are used.

---

### Complexity

- **Time:** O(n²)
- **Space:** O(1)

---

## 2. Better Approach (Hash Map + Sorting)

### Idea

Store every character's frequency using a hash map.

Then store the `(frequency, character)` pairs in a vector.

Sort the vector in descending order of frequency.

Finally, build the answer.

---

### Algorithm

1. Count frequencies using a hash map.
2. Store `(frequency, character)` pairs.
3. Sort the vector.
4. Append each character `frequency` times.
5. Return the answer.

---

## Code (Hash Map + Sorting)

```cpp
class Solution {
public:
    string frequencySort(string s) {

        // Stores frequency of every character.
        unordered_map<char, int> freq;

        // Count occurrences.
        for(char ch : s)
            freq[ch]++;

        // Store (frequency, character) pairs.
        vector<pair<int, char>> vec;

        for(auto it : freq) {
            vec.push_back({it.second, it.first});
        }

        // Sort by frequency in descending order.
        sort(vec.begin(), vec.end(), greater<pair<int,char>>());

        string ans = "";

        // Add each character frequency number of times.
        for(auto it : vec) {
            ans.append(it.first, it.second);
        }

        return ans;
    }
};
```

---

### Complexity

- **Time:** O(n log n)
- **Space:** O(n)

---

## 3. Optimal Approach (Bucket Sort)

### Idea

The maximum possible frequency of any character is `n`.

Instead of sorting,

create buckets where

```text
bucket[i]
```

stores all characters having frequency `i`.

Finally,

traverse buckets from highest frequency to lowest.

---

### Algorithm

1. Count frequencies.
2. Create buckets.
3. Put each character into its frequency bucket.
4. Traverse buckets backwards.
5. Append characters.

---

## Code (Optimal)

```cpp
class Solution {
public:
    string frequencySort(string s) {

        // Stores frequency of every character.
        unordered_map<char, int> freq;

        // Count occurrences of each character.
        for(char ch : s) {
            freq[ch]++;
        }

        // bucket[i] stores all characters
        // whose frequency is exactly i.
        vector<vector<char>> bucket(s.size() + 1);

        // Place every character into its bucket.
        for(auto it : freq) {
            bucket[it.second].push_back(it.first);
        }

        string ans = "";

        // Traverse from highest frequency
        // to lowest frequency.
        for(int i = s.size(); i >= 1; i--) {

            // Multiple characters can have
            // the same frequency.
            for(char ch : bucket[i]) {

                // Add the character exactly
                // i times.
                ans.append(i, ch);
            }
        }

        return ans;
    }
};
```

---

### Complexity

- **Time:** O(n)
- **Space:** O(n)

---

# Understanding `ans.append(i, ch)`

Suppose

```cpp
ans.append(4, 'a');
```

Result

```text
aaaa
```

Similarly,

```cpp
ans.append(3, 'x');
```

becomes

```text
xxx
```

This is much shorter than writing

```cpp
for(int j = 0; j < i; j++)
    ans += ch;
```

---

# Dry Run

Input

```text
tree
```

---

### Frequency Map

```text
t → 1

r → 1

e → 2
```

---

### Buckets

```text
Bucket[1]

t r
```

```text
Bucket[2]

e
```

---

### Traverse Backwards

Frequency

```text
2
```

Append

```text
ee
```

Answer

```text
ee
```

---

Frequency

```text
1
```

Append

```text
t

r
```

Final

```text
eert
```

---

# Why Bucket Sort Works

Suppose

```text
abbcccdddd
```

Frequency

```text
a → 1

b → 2

c → 3

d → 4
```

Buckets become

```text
1 → a

2 → b

3 → c

4 → d
```

Traversing backwards naturally gives

```text
dddd

ccc

bb

a
```

Exactly what we need.

---

# Interview Tips

### ✅ Observation 1

The problem does **not** ask for alphabetical sorting.

Only frequency matters.

---

### ✅ Observation 2

Characters with equal frequency can appear in **any order**.

So don't waste time maintaining alphabetical order.

---

### ✅ Observation 3

Whenever frequency ranges from

```text
1 to n
```

think about **Bucket Sort**.

It often reduces

```text
O(n log n)

↓

O(n)
```

---

# Understanding the Optimal Approach (Bucket Sort)

Before understanding the code, let's first understand **what Bucket Sort means**.

---

## What is Bucket Sort?

Bucket Sort is a sorting technique where we **group elements into buckets instead of comparing them with each other.**

Normally, when we sort numbers:

```text
5 1 3 2 4
```

we compare numbers.

But in Bucket Sort, we ask:

> "Which bucket should this element belong to?"

---

### Example

Suppose frequencies are

```text
a -> 1

b -> 3

c -> 2

d -> 3
```

Instead of sorting these frequencies, we create buckets.

```text
Bucket[1]

a
```

```text
Bucket[2]

c
```

```text
Bucket[3]

b d
```

Now simply traverse buckets from largest frequency to smallest.

```text
Bucket[3]

↓

Bucket[2]

↓

Bucket[1]
```

Output

```text
bbbddcca
```

(or any order among characters having same frequency)

Notice

**No sorting is performed.**

---

# Why Bucket Sort Works Here

Suppose

```text
s = "tree"
```

Frequency table

```text
t -> 1

r -> 1

e -> 2
```

Maximum frequency can never exceed

```text
length of string = 4
```

So we create

```cpp
vector<vector<char>> bucket(5);
```

Index

```text
0

1

2

3

4
```

Initially

```text
0 :

1 :

2 :

3 :

4 :
```

Insert characters.

```text
t

↓

bucket[1]
```

```text
r

↓

bucket[1]
```

```text
e

↓

bucket[2]
```

Buckets become

```text
0 :

1 : t r

2 : e

3 :

4 :
```

Now traverse backwards.

```text
4

↓

3

↓

2

↓

1
```

When we reach

```text
bucket[2]
```

append

```text
ee
```

When we reach

```text
bucket[1]
```

append

```text
t

r
```

Answer

```text
eert
```

---

# Why Is It O(n)?

Building frequency map

```text
O(n)
```

Putting characters into buckets

```text
O(n)
```

Traversing buckets

```text
O(n)
```

Total

```text
O(n)
```

No sorting is performed.

---

# Code Explained Line by Line (Hash Map + Sorting)

```cpp
class Solution {
public:
    string frequencySort(string s) {
```

We are given a string and need to return the characters arranged in decreasing order of frequency.

---

```cpp
unordered_map<char, int> freq;
```

Create a **hash map**.

Think of it as a dictionary.

Initially

```text
{}
```

After processing

```text
tree
```

it becomes

```text
{
t : 1

r : 1

e : 2
}
```

The key is the character.

The value is its frequency.

---

```cpp
for(char ch : s)
    freq[ch]++;
```

Loop through every character.

Example

```text
tree
```

Iteration 1

```text
t

freq[t] = 1
```

Iteration 2

```text
r

freq[r] = 1
```

Iteration 3

```text
e

freq[e] = 1
```

Iteration 4

```text
e

freq[e] = 2
```

Final map

```text
t : 1

r : 1

e : 2
```

---

```cpp
vector<pair<int,char>> vec;
```

Create a vector.

Each element stores

```text
(frequency, character)
```

Example

```text
(2,e)

(1,t)

(1,r)
```

---

```cpp
for(auto it : freq)
```

Visit every element of the map.

Suppose

```text
it

↓

{e,2}
```

---

```cpp
vec.push_back({it.second,it.first});
```

Remember

```text
it.first

↓

character
```

```text
it.second

↓

frequency
```

So

```text
(e,2)
```

becomes

```text
(2,e)
```

Why?

Because we want to sort by frequency.

Now

```text
vec

↓

[(2,e),(1,t),(1,r)]
```

---

```cpp
sort(vec.begin(),vec.end(),greater<pair<int,char>>());
```

Sort descending.

Since pair stores

```text
(frequency,character)
```

sorting automatically compares frequency first.

Result

```text
(2,e)

(1,t)

(1,r)
```

---

```cpp
string ans="";
```

Create answer string.

Initially

```text
""
```

---

```cpp
for(auto it : vec)
```

Visit every pair.

First

```text
(2,e)
```

---

```cpp
ans.append(it.first,it.second);
```

Remember

```text
append(count,character)
```

Example

```cpp
ans.append(2,'e');
```

becomes

```text
ee
```

Then

```cpp
append(1,'t');
```

becomes

```text
eet
```

Then

```cpp
append(1,'r');
```

becomes

```text
eetr
```

Done.

---

```cpp
return ans;
```

Return final string.

---

# Complete Dry Run (Sorting Approach)

Input

```text
tree
```

Frequency map

```text
t : 1

r : 1

e : 2
```

Vector

```text
[(2,e),(1,t),(1,r)]
```

Sorted

```text
[(2,e),(1,t),(1,r)]
```

Build

```text
ee

↓

eet

↓

eetr
```

Answer

```text
eetr
```

---

# Code Explained Line by Line (Bucket Sort)

```cpp
unordered_map<char,int> freq;
```

Exactly the same as before.

Stores frequencies.

---

```cpp
for(char ch:s)
    freq[ch]++;
```

Frequency map

```text
t : 1

r : 1

e : 2
```

---

```cpp
vector<vector<char>> bucket(s.size()+1);
```

This is the most important line.

Suppose

```text
tree
```

Length

```text
4
```

Create

```text
bucket[0]

bucket[1]

bucket[2]

bucket[3]

bucket[4]
```

Each bucket is itself a vector.

So

```text
bucket[2]
```

can store many characters.

---

Initially

```text
0 :

1 :

2 :

3 :

4 :
```

---

```cpp
for(auto it:freq)
```

Visit

```text
(e,2)
```

---

```cpp
bucket[it.second].push_back(it.first);
```

Remember

```text
it.second

↓

frequency
```

```text
it.first

↓

character
```

So

```text
bucket[2].push_back('e');
```

Now

```text
Bucket[2]

↓

e
```

Next

```text
t

↓

bucket[1]
```

Next

```text
r

↓

bucket[1]
```

Buckets become

```text
0 :

1 : t r

2 : e

3 :

4 :
```

---

```cpp
string ans="";
```

Create answer.

---

```cpp
for(int i=s.size();i>=1;i--)
```

Start from

```text
4

↓

3

↓

2

↓

1
```

Why?

Because higher frequency should come first.

---

Suppose

```text
i=2
```

Bucket

```text
e
```

---

```cpp
for(char ch:bucket[i])
```

Visit every character having frequency

```text
2
```

Only

```text
e
```

---

```cpp
ans.append(i,ch);
```

Means

```cpp
append(2,'e');
```

Result

```text
ee
```

Later

```text
i=1
```

Characters

```text
t

r
```

Append

```text
t

↓

r
```

Final

```text
eetr
```

---

# Dry Run (Bucket Sort)

Frequency

```text
t : 1

r : 1

e : 2
```

Buckets

```text
0 :

1 : t r

2 : e

3 :

4 :
```

Traverse

```text
4

↓

3

↓

2

↓

1
```

Output

```text
ee

↓

eet

↓

eetr
```

Done.

---

# Which One Should You Use?

| Approach | Time | Space | Interview Preference |
|-----------|------|--------|----------------------|
| Hash Map + Sorting | O(n log n) | O(n) | ⭐⭐⭐⭐ Easy to explain |
| Bucket Sort | O(n) | O(n) | ⭐⭐⭐⭐⭐ Best/Optimal |

---

## Interview Tip ⭐

If you cannot immediately think of Bucket Sort in an interview, **write the Hash Map + Sorting solution first**. It's clean, accepted, and demonstrates the correct logic. Then mention:

> "Since the maximum possible frequency is at most `n`, we can optimize the sorting step using Bucket Sort to achieve **O(n)** time."

Interviewers appreciate this progression because it shows both problem-solving ability and optimization skills.