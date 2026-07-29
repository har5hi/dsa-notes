# LeetCode 14 - Longest Common Prefix

---

# Problem Statement

Write a function to find the **longest common prefix** string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

---

# Approaches

## 1. Brute Force (Character-by-Character Comparison)

### Idea

Use the first string as a reference.

For every character in the first string:

- Compare it with the same position in every other string.
- If all strings have the same character, include it in the answer.
- Otherwise, stop immediately.

---

### Algorithm

1. Take the first string.
2. Traverse each character.
3. Compare that character with every other string.
4. If mismatch or string ends:
   - Return answer.
5. Otherwise continue.

---

### Code

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {

        if(strs.empty())
            return "";

        string ans = "";

        for(int i = 0; i < strs[0].size(); i++) {
            char ch = strs[0][i];

            for(int j = 1; j < strs.size(); j++) {
                if(i >= strs[j].size() || strs[j][i] != ch)
                    return ans;
            }
            ans += ch;
        }
        return ans;
    }
};
```

---

### Complexity

- **Time:** O(n × m)

Where

- `n` = number of strings
- `m` = length of the shortest string

- **Space:** O(1)

---

## 2. Better Approach (Progressively Shrink Prefix)

### Idea

Assume the first string is the current prefix.

Compare it with every string.

Whenever a mismatch occurs, shorten the prefix until both match.

Eventually, the remaining prefix is common to all strings.

---

### Algorithm

1. Initialize prefix as first string.
2. Compare prefix with every string.
3. While current string doesn't start with prefix:
   - Remove last character of prefix.
4. Return prefix.

---

### Code

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {

        string prefix = strs[0];

        for(int i = 1; i < strs.size(); i++) {

            while(strs[i].find(prefix) != 0) {

                prefix.pop_back();

                if(prefix.empty())
                    return "";
            }
        }

        return prefix;
    }
};
```

---

### Complexity

- **Time:** O(n × m)
- **Space:** O(1)

---

## 3. Optimal Approach (Sorting)

### Idea

After sorting the strings lexicographically:

- The most different strings become the **first** and **last** strings.
- Any common prefix shared by all strings must also be shared by these two.

So we only compare:

- First string
- Last string

---

### Example

```text
Input:

["flower","flow","flight"]

After sorting:

["flight","flow","flower"]
```

Compare

```text
flight
flower
```

Common prefix

```text
fl
```

---

### Algorithm

1. Sort the array.
2. Compare first and last strings.
3. Keep matching characters.
4. Return prefix.

---

### Code (Optimal)

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {

        sort(strs.begin(), strs.end());

        string first = strs.front();
        string last = strs.back();

        string ans = "";

        for(int i = 0; i < min(first.size(), last.size()); i++) {

            if(first[i] != last[i])
                break;

            ans += first[i];
        }
        return ans;
    }
};
```

---

### Complexity

- **Time:** O(n log n × m)
- **Space:** O(1)

> **Note:** Although elegant, sorting is **not the fastest** because of the `O(n log n)` sorting cost. The character-by-character comparison is generally preferred in interviews due to its linear scan.

---

# Dry Run

Input

```text
["flower","flow","flight"]
```

Take

```text
flower
```

Compare

```text
f ✔

l ✔

o ✖ (flight has 'i')
```

Answer

```text
fl
```

---

This is a very clever solution based on sorting. The key observation is:

After sorting the strings lexicographically, the longest common prefix of the entire array is the same as the common prefix of the first and last strings.

Let's understand it line by line.

Code
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {

        sort(strs.begin(), strs.end());

        string first = strs.front();
        string last = strs.back();

        string ans = "";

        for(int i = 0; i < min(first.size(), last.size()); i++) {

            if(first[i] != last[i])
                break;

            ans += first[i];
        }
        return ans;
    }
};
Step 1: Sort the array
sort(strs.begin(), strs.end());

This sorts the strings in dictionary (lexicographical) order.

Example

Before sorting

["flower","flow","flight"]

After sorting

["flight","flow","flower"]

Notice:

flight comes first.
flower comes last.

Another example

["dog","racecar","car"]

After sorting

["car","dog","racecar"]
Why do we sort?

Sorting places similar strings together.

More importantly,

the smallest string comes first
the largest string comes last

These two strings are the most different lexicographically.

If these two share a prefix,

then every string between them must also share that prefix.

This is the entire trick.

Step 2: Store first and last strings
string first = strs.front();
string last = strs.back();

Equivalent to

string first = strs[0];
string last = strs[strs.size()-1];

For

["flight","flow","flower"]

we get

first = "flight"

last = "flower"
Step 3: Create answer string
string ans = "";

Initially

ans = ""
Step 4: Compare characters
for(int i = 0; i < min(first.size(), last.size()); i++)

Why

min(first.size(), last.size())

?

Suppose

first = "cat"

last = "catalog"

We cannot go beyond

3

because the first string ends.

Step 5: Check mismatch
if(first[i] != last[i])
    break;

The moment characters differ,

the common prefix ends.

Example

flight

flower

Compare

f == f ✔

l == l ✔

i != o ✖

Stop.

Step 6: Add matching character
ans += first[i];

After each match

ans = "f"

↓

ans = "fl"

Finally

return "fl"
Complete Dry Run

Input

["flower","flow","flight"]
After sorting
["flight","flow","flower"]
first = "flight"

last = "flower"
Loop
i = 0
first[0] = 'f'

last[0] = 'f'

Match

ans = "f"
i = 1
l == l
ans = "fl"
i = 2
i != o

Break.

Return

fl
Another Example

Input

["apple","ape","application"]

After sorting

["ape","apple","application"]

Compare

ape

application
a == a ✔

p == p ✔

e != p ✖

Answer

ap

Notice we never even looked at "apple".

Why comparing only first and last works

Suppose after sorting we have

ape

apple

application

If

ape

and

application

agree on

ap

then

every string lying between them alphabetically must also start with ap.

If some middle string didn't start with ap, it would no longer fall between them in sorted order.

This property is why sorting works.

Complexity
Sorting
sort(...)

takes

O(n log n)

where n is the number of strings.

Comparing first and last takes

O(m)

where m is the length of the shortest string.

Overall:

Time: O(n log n × m) (string comparisons during sorting involve characters)

Space: O(1) (ignoring the space used by the sorting algorithm).

Interview Perspective

This is a neat and concise solution, but it's not the most efficient.

The character-by-character approach (comparing the first string against all others) runs in O(n × m) and avoids the O(n log n) sorting cost.

If asked in an interview:

Mention the sorting approach as an elegant observation.
Then say the preferred solution is the direct character comparison because it achieves linear scanning without sorting. This shows you know both the clever trick and the optimal algorithm.