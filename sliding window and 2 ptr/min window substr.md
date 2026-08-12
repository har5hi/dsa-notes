# LeetCode 76 - Minimum Window Substring

**Difficulty:** Hard

**Topic:** String, HashMap, Sliding Window, Two Pointers

---

# Problem Statement

Given two strings

- `s`
- `t`

Return the **minimum window substring** of `s` such that **every character of `t` (including duplicates)** is present in the window.

If no such window exists,

return

```text
""
```

If multiple answers exist,

return any one of them.

---

# Example 1

```text
Input

s = "ADOBECODEBANC"

t = "ABC"
```

Output

```text
"BANC"
```

Explanation

"BANC" is the smallest substring containing

```
A

B

C
```

---

# Example 2

```text
Input

s = "a"

t = "a"
```

Output

```text
"a"
```

---

# Example 3

```text
Input

s = "a"

t = "aa"
```

Output

```text
""
```

Explanation

One 'a' is not enough because

`t`

needs

```
2
```

copies of

```
'a'
```

---

# Understanding the Problem

Unlike previous sliding window problems,

we are **NOT counting** substrings.

We are **NOT finding the longest** substring.

Instead,

we need to find the

```text
Smallest Window
```

that contains **every character of `t`**.

---

# Example

```
s = ADOBECODEBANC

t = ABC
```

Possible windows

```
ADOBEC

↓

DOBECODEBA

↓

BANC
```

Among all valid windows,

return the **smallest**.

---

# Important Observation

A window is **valid** only if it contains

every character of `t`

with the **correct frequency**.

Example

```
t = AABC
```

Window

```
ABCA
```

Valid

because

```
A → 2

B → 1

C → 1
```

Window

```
ABC
```

Invalid

because

```
A → 1

Need

2
```

So frequencies matter.

---

# Approaches

---

# 1. Brute Force

## Intuition

Generate every possible substring.

For each substring,

count character frequencies.

Check whether it contains

all characters of `t`.

Keep the smallest valid substring.

---

# Algorithm

1. Generate every substring.
2. Count frequencies.
3. Compare with `t`.
4. If valid,
   update minimum length.
5. Return smallest substring.

---

# Brute Force Code

```cpp
class Solution {
public:

    string minWindow(string s, string t) {

        int n = s.size();

        string ans = "";

        int minLen = INT_MAX;

        for(int i = 0; i < n; i++) {

            vector<int> freq(128,0);

            for(int j = i; j < n; j++) {

                freq[s[j]]++;

                bool ok = true;

                vector<int> need(128,0);

                for(char ch : t)
                    need[ch]++;

                for(int k = 0; k < 128; k++) {

                    if(freq[k] < need[k]) {

                        ok = false;
                        break;
                    }
                }

                if(ok) {

                    if(j-i+1 < minLen) {

                        minLen = j-i+1;
                        ans = s.substr(i,minLen);
                    }
                }
            }
        }

        return ans;
    }
};
```

---

# Complexity

### Time

```text
O(n³)
```

Generating every substring

+

checking frequencies.

---

### Space

```text
O(1)
```

---

# Why Brute Force is Slow?

Suppose

```
ADOBECODEBANC
```

It checks

```
A

↓

AD

↓

ADO

↓

ADOB

↓

...
```

Then starts again

```
D

↓

DO

↓

DOB

...
```

Most work is repeated.

---

# 2. Better Approach

Precompute

frequency of `t`.

For every starting index,

expand until all required characters are found.

Still

```
O(n²)
```

Not efficient enough.

---

# 3. Optimal Approach (Sliding Window)

⭐ Most Interview Preferred

---

# Intuition

Maintain a sliding window.

Expand the window until it becomes

```text
VALID
```

Once valid,

shrink it from the left

to make it as small as possible.

While shrinking,

keep updating the answer.

This is different from previous problems.

Earlier,

we wanted

```
Largest Window
```

or

```
Count of Windows
```

Here,

we want

```
Smallest Valid Window
```

---

# Key Idea

The window has two states.

### Invalid

```
A D O

Missing

B

C
```

Expand.

---

### Valid

```
A D O B E C
```

Contains

```
A

B

C
```

Now,

instead of expanding,

start shrinking.

---

# Visualization

```
Expand →

A D O B E C O D E B A N C
^
L

                    ^
                    R
```

Window becomes valid.

Now

```
Shrink ←
```

until it becomes invalid again.

Repeat.

---

# Algorithm

1. Store frequency of every character in `t`.
2. Expand the window using `right`.
3. Update current window frequencies.
4. If the window becomes valid,
   try shrinking from the left.
5. Keep updating the smallest valid window.
6. Return the answer.

---

# Optimal Code (Fully Commented)

```cpp
class Solution {
public:

    string minWindow(string s, string t) {

        vector<int> need(128,0);

        // Store frequency of characters required
        for(char ch : t)
            need[ch]++;

        int left = 0;

        int count = 0;

        int start = 0;

        int minLen = INT_MAX;

        for(int right = 0; right < s.size(); right++) {

            // Include current character
            if(need[s[right]] > 0)
                count++;

            need[s[right]]--;

            // Window contains every required character
            while(count == t.size()) {

                // Update minimum window
                if(right-left+1 < minLen) {

                    minLen = right-left+1;
                    start = left;
                }

                // Remove left character
                need[s[left]]++;

                if(need[s[left]] > 0)
                    count--;

                left++;
            }
        }

        if(minLen == INT_MAX)
            return "";

        return s.substr(start,minLen);
    }
};
```

---

# Complexity

### Time

```text
O(n)
```

Every character

- enters the window once
- leaves the window once

---

### Space

```text
O(1)
```

Only a frequency array of size

```
128
```

is used.

---

# Why Sliding Window Works

The window expands until it becomes valid.

Once valid,

expanding further only makes it larger.

Since we need the **smallest** valid window,

we immediately start shrinking it.

This process continues until the window becomes invalid again.

Thus,

both pointers move only forward,

giving an overall complexity of

```text
O(n)
```

---

# Understanding the Optimal Solution (Sliding Window)

This is one of the most difficult Sliding Window problems initially.

But once you understand the logic,

it becomes one of the easiest templates to remember.

---

# The Main Idea

Unlike previous questions,

we don't care about

- maximum window
- number of windows

Instead,

we only care about

```text
Smallest Valid Window
```

So our strategy becomes

```text
Expand Window

↓

Become Valid

↓

Shrink Window

↓

Update Minimum

↓

Become Invalid

↓

Expand Again
```

This cycle continues until the string ends.

---

# Complete Code

```cpp
class Solution {
public:

    string minWindow(string s, string t) {

        vector<int> need(128,0);

        // Store frequency of every character in t
        for(char ch : t)
            need[ch]++;

        int left = 0;

        // Number of required characters matched
        int count = 0;

        int start = 0;

        int minLen = INT_MAX;

        for(int right = 0; right < s.size(); right++) {

            // If this character is still needed
            if(need[s[right]] > 0)
                count++;

            // Include current character
            need[s[right]]--;

            // Window contains every required character
            while(count == t.size()) {

                // Update answer
                if(right-left+1 < minLen) {
                    minLen = right-left+1;
                    start = left;
                }

                // Remove left character
                need[s[left]]++;

                // Removing a required character?
                if(need[s[left]] > 0)
                    count--;

                left++;
            }
        }

        if(minLen == INT_MAX)
            return "";

        return s.substr(start,minLen);
    }
};
```

---

# Understanding Every Line

---

## Line 1

```cpp
vector<int> need(128,0);
```

Creates a frequency array.

Why size 128?

ASCII characters range from

```
0

to

127
```

Each index represents one character.

Initially

```
All values = 0
```

---

## Line 2

```cpp
for(char ch : t)
    need[ch]++;
```

Store how many times each character is needed.

Example

```
t = "AABC"
```

After loop

```
A → 2

B → 1

C → 1
```

Everything else

```
0
```

---

## Why do we store frequencies?

Suppose

```
t = AABC
```

Window

```
ABC
```

Contains

```
A

B

C
```

Still invalid.

Why?

Because

```
Need

2

A's
```

Frequency solves this problem.

---

## Line 3

```cpp
int left = 0;
```

Beginning of the sliding window.

Initially

```
A D O B ...

^

left
```

---

## Line 4

```cpp
int count = 0;
```

This is the most confusing variable.

Let's understand it carefully.

---

# What does count mean?

It is **NOT** the number of distinct characters.

It is **NOT** the window size.

It means

```text
How many required characters have been matched.
```

Example

```
t = ABC
```

Need

```
A

B

C
```

Window

```
A D O
```

Matched

```
A
```

```
count = 1
```

Window

```
A D O B
```

Matched

```
A

B
```

```
count = 2
```

Window

```
A D O B E C
```

Matched

```
A

B

C
```

```
count = 3
```

Since

```
t.size() = 3
```

Window becomes valid.

---

## Another Example

Suppose

```
t = AABC
```

Need

```
A A B C
```

Window

```
A B C
```

Matched

```
A

B

C
```

Only

```
3
```

characters matched.

Still missing one

```
A
```

So

```
count = 3

t.size() = 4
```

Window is NOT valid.

---

## Line 5

```cpp
int start = 0;
```

Stores where the minimum window starts.

---

## Line 6

```cpp
int minLen = INT_MAX;
```

Initially,

we haven't found any valid window.

So minimum length is

```
∞
```

---

## Line 7

```cpp
for(int right = 0; right < s.size(); right++)
```

Expand the window.

Example

```
A

↓

A D

↓

A D O

↓

A D O B
```

---

# Most Important Part

```cpp
if(need[s[right]] > 0)
    count++;
```

Suppose

Need

```
A → 1

B → 1

C → 1
```

Current character

```
A
```

Need

```
A = 1
```

Positive.

Meaning

```
A is still required.
```

So

```
count++
```

Now

```
count = 1
```

---

## Why only when

```cpp
need > 0
```

Suppose

Need

```
A → 0
```

Means

```
Already enough A's
```

Another

```
A
```

comes.

Need

```
0
```

Not positive.

Don't increase count.

Otherwise,

extra characters would be counted incorrectly.

---

## Next Line

```cpp
need[s[right]]--;
```

This line confuses almost everyone.

Let's understand.

---

Suppose

Need

```
A = 1
```

Window takes

```
A
```

Now

Need

```
0
```

Means

```
Requirement satisfied.
```

---

Suppose another

```
A
```

comes.

Need becomes

```
-1
```

Negative means

```
Extra A

Not required

But harmless.
```

---

### Meaning of need values

Positive

```
Still required
```

Zero

```
Exactly satisfied
```

Negative

```
Extra copies
```

This interpretation is extremely important.

---

# The While Loop

```cpp
while(count == t.size())
```

When

```
count == size of t
```

the window contains every required character.

Now,

instead of expanding,

start shrinking.

---

## Update Answer

```cpp
if(right-left+1 < minLen)
```

Current window

```
left            right

↓

A D O B E C
```

Length

```
6
```

If smaller than previous,

store it.

---

## Remove Left Character

```cpp
need[s[left]]++;
```

Suppose

```
A
```

leaves.

Need

```
0

↓

1
```

Meaning

```
Now A is required again.
```

---

## Why

```cpp
if(need[s[left]] > 0)
```

Suppose

Need becomes

```
1
```

Means

```
We are missing one required character.
```

Window becomes invalid.

Decrease

```
count
```

---

Suppose instead

Need

```
-1

↓

0
```

Still enough copies remain.

Window is still valid.

Don't reduce count.

---

## Move Left

```cpp
left++;
```

Shrink the window.

---

# Complete Dry Run

Input

```
s = ADOBECODEBANC

t = ABC
```

Need

```
A = 1

B = 1

C = 1
```

```
count = 0
```

---

### right = A

Need[A]

```
1
```

Positive

```
count = 1
```

Need becomes

```
0
```

---

### right = D

Need[D]

```
0
```

No change.

Need becomes

```
-1
```

---

### right = O

Same.

---

### right = B

Need[B]

```
1
```

Positive

```
count = 2
```

Need becomes

```
0
```

---

### right = E

Nothing.

---

### right = C

Need[C]

```
1
```

Positive

```
count = 3
```

Now

```
count == t.size()
```

Window

```
ADOBEC
```

Valid.

Update answer.

Start shrinking.

---

Remove

```
A
```

Need[A]

```
0

↓

1
```

Positive.

Missing A again.

```
count = 2
```

Window becomes invalid.

Expand again.

---

Eventually

window becomes

```
BANC
```

Length

```
4
```

Smallest possible.

Answer

```
BANC
```

---

# Visual Representation

```
Expand →

A D O B E C O D E B A N C
^
L

                    ^
                    R
```

Window becomes valid.

Now

```
Shrink ←
```

```
D O B E C O D E B A N C

^

L
```

Invalid again.

Expand.

Repeat until end.

---

# Why This Works

The window only does two things:

```
Expand

↓

Until Valid

↓

Shrink

↓

Until Invalid

↓

Expand Again
```

Each pointer (`left` and `right`) moves only forward, never backward.

So every character is visited at most twice:

- once when `right` includes it,
- once when `left` removes it.

Therefore:

- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(1)` (fixed-size frequency array)

---

# Common Mistakes

---

## ❌ Mistake 1: Using a Set instead of a Frequency Array

Many beginners try

```cpp
unordered_set<char> st;
```

This is **wrong**.

Why?

Because duplicates matter.

Example

```
t = "AABC"
```

Window

```
ABC
```

A set says

```
A
B
C
```

Looks valid.

But actually

```
Need

2 A's
```

So the window is invalid.

That's why we use a **frequency array/map** instead of a set.

---

## ❌ Mistake 2: Thinking `count` stores distinct characters

Wrong.

Many students think

```cpp
count = number of distinct characters
```

No.

It stores

```text
Number of required characters matched.
```

Example

```
t = "AABC"
```

Need

```
A A B C
```

Window

```
A B C
```

Distinct

```
3
```

Matched characters

```
3
```

Still invalid because

```
Need one more A
```

So

```
count = 3

t.size() = 4
```

---

## ❌ Mistake 3: Using

```cpp
while(count >= t.size())
```

Wrong.

Correct

```cpp
while(count == t.size())
```

Because

```
count

can never become

greater than t.size()
```

Every matched character is counted only once.

---

## ❌ Mistake 4: Forgetting

```cpp
need[s[right]]--;
```

Some people write

```cpp
if(need[s[right]] > 0)
    count++;
```

and stop.

Wrong.

Every character entering the window must decrease its requirement.

Otherwise,

the frequency array never changes.

---

## ❌ Mistake 5: Forgetting

```cpp
need[s[left]]++;
```

When removing a character,

its requirement must increase again.

Otherwise,

the algorithm never realizes that the character is missing.

---

## ❌ Mistake 6: Returning the Window Instead of the Substring

Wrong

```cpp
return minLen;
```

Correct

```cpp
return s.substr(start, minLen);
```

Because the problem asks for

```text
The substring
```

not its length.

---

## ❌ Mistake 7: Not Handling "No Answer"

Example

```
s = "ABC"

t = "XYZ"
```

No valid window exists.

Always check

```cpp
if(minLen == INT_MAX)
    return "";
```

---

# Frequently Asked Interview Questions

---

## Q1. Why don't we use a HashSet?

Because duplicates matter.

```
AABC
```

and

```
ABC
```

have the same set,

but different frequencies.

---

## Q2. Why use a Frequency Array instead of a HashMap?

Characters are ASCII.

ASCII contains only

```
128
```

characters.

Array lookup

```
O(1)
```

is faster than HashMap lookup.

---

## Q3. Why is

```cpp
need[s[right]]--;
```

done even when the character isn't required?

Suppose

```
Need

A = 0
```

Another

```
A
```

comes.

Need becomes

```
-1
```

Negative simply means

```
Extra copies available.
```

Later,

while shrinking,

these extra copies can be removed without making the window invalid.

---

## Q4. Why do we shrink immediately after finding a valid window?

Because the goal is

```text
Minimum Window
```

A larger valid window is never better than a smaller valid window.

So as soon as the window becomes valid,

we try to make it smaller.

---

## Q5. Why does this algorithm run in O(n)?

Because

```
Left pointer

moves only forward

Right pointer

moves only forward
```

Each character

- enters once
- leaves once

Maximum operations

```
2n
```

Therefore

```
O(n)
```

---

# Comparison with Other Sliding Window Problems

## LC 3 — Longest Substring Without Repeating Characters

Goal

```text
Largest Valid Window
```

Update

```cpp
maxLen = max(maxLen, right-left+1);
```

---

## LC 424 — Longest Repeating Character Replacement

Goal

```text
Largest Valid Window
```

Update

```cpp
maxLen = max(maxLen, right-left+1);
```

---

## LC 992 — Subarrays With K Different Integers

Goal

```text
Count Windows
```

Update

```cpp
ans += right-left+1;
```

---

## LC 1358 — Number of Substrings Containing ABC

Goal

```text
Count Windows
```

Update

```cpp
ans += n-right;
```

---

## LC 76 — Minimum Window Substring

Goal

```text
Smallest Valid Window
```

Update

```cpp
if(right-left+1 < minLen)
{
    minLen = right-left+1;
    start = left;
}
```

This is a completely different sliding window pattern.

---

# Sliding Window Pattern Summary

## Pattern 1

Longest Valid Window

```cpp
maxLen = max(maxLen, right-left+1);
```

Examples

- LC 3
- LC 424
- LC 1004
- LC 904

---

## Pattern 2

Count Valid Windows

```cpp
ans += right-left+1;
```

Examples

- LC 930
- LC 1248
- LC 992

---

## Pattern 3

Count Remaining Windows

```cpp
ans += n-right;
```

Examples

- LC 1358

---

## Pattern 4

Minimum Valid Window

```cpp
if(window is valid)
{
    update minimum

    shrink
}
```

Example

- LC 76 ⭐

---

## Pattern 5

Fixed Size Sliding Window

Window size never changes.

Examples

- LC 1423
- LC 643

---

# Interview Tips

### ⭐ Tip 1

Whenever the question says

```text
Smallest

Minimum

Shortest
```

Think

```
Expand

↓

Become Valid

↓

Shrink Immediately
```

---

### ⭐ Tip 2

If duplicates matter,

never use a set.

Always use

```cpp
Frequency Array

or

HashMap
```

---

### ⭐ Tip 3

If the problem asks for

```
Contains all characters
```

look for

```cpp
Frequency Counting
```

instead of just checking existence.

---

### ⭐ Tip 4

Remember what each common sliding window update means:

```cpp
max(maxLen, windowLength)
```

➡️ Longest valid window

```cpp
ans += right-left+1
```

➡️ Count all valid windows ending at `right`

```cpp
ans += n-right
```

➡️ Count all valid windows starting at `left`

```cpp
min(windowLength)
```

➡️ Smallest valid window

---

# Final Cheat Sheet

| Pattern | Goal | Update Statement | Example Problems |
|---------|------|------------------|------------------|
| Longest Valid Window | Maximum length | `maxLen = max(maxLen, right-left+1)` | LC 3, 424, 904, 1004 |
| Count Valid Windows | Count subarrays/substrings | `ans += right-left+1` | LC 930, 1248, 992 |
| Count Remaining Windows | Count using current left | `ans += n-right` | LC 1358 |
| Minimum Valid Window | Smallest substring | Update answer, then shrink | **LC 76** |
| Fixed Size Window | Window length fixed | Slide by adding & removing one element | LC 1423, LC 643 |

---

# Key Takeaway

The biggest lesson from **LC 76** is:

> **When the window first becomes valid, don't stop. Start shrinking it immediately to find the smallest valid window.**

Unlike most sliding window problems where we maximize the window or count windows, here the objective is to **minimize** the window while preserving validity.

This "expand until valid → shrink while valid" pattern is one of the most frequently tested sliding window techniques in coding interviews and is essential to master.