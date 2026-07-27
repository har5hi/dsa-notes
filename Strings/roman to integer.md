# LeetCode 13 - Roman to Integer

---

# Problem Statement

Roman numerals are represented by seven symbols:

| Symbol | Value |
|--------|------:|
| I | 1 |
| V | 5 |
| X | 10 |
| L | 50 |
| C | 100 |
| D | 500 |
| M | 1000 |

Given a Roman numeral string, convert it into an integer.

Roman numerals are usually written from **largest to smallest**, but there are six special cases where subtraction is used.

These are:

```text
IV = 4

IX = 9

XL = 40

XC = 90

CD = 400

CM = 900
```

---

# Understanding Roman Numerals

Normally,

Roman numerals are written from larger values to smaller values.

Example

```text
VIII

=

5 + 1 + 1 + 1

=

8
```

Easy.

---

But sometimes subtraction is used.

Example

```text
IV
```

does NOT mean

```text
1 + 5 = 6
```

Instead,

because

```text
I comes before V
```

it means

```text
5 - 1 = 4
```

Similarly

```text
IX

=

10 - 1

=

9
```

---

# Key Observation

Suppose we have

```text
VI
```

Values

```text
5

1
```

Since

```text
5 > 1
```

Simply add.

Answer

```text
6
```

---

Now consider

```text
IV
```

Values

```text
1

5
```

Since

```text
1 < 5
```

This is a subtraction case.

Instead of adding,

subtract.

---

Therefore,

Whenever

```text
Current Value < Next Value
```

↓

Subtract.

Otherwise

↓

Add.

This single observation solves the problem.

---

# Approaches

---

# 1. Brute Force

## Idea

Check every possible subtraction pair manually.

For example,

```text
IV

IX

XL

XC

CD

CM
```

Whenever one is found,

add its value and skip two characters.

Otherwise,

add the normal Roman value.

---

## Algorithm

1. Traverse the string.
2. Check if current and next character form one of the six special cases.
3. If yes

- Add special value.
- Skip next character.

4. Else

- Add normal value.

---

## Code

```cpp
class Solution {
public:

    int value(char ch){

        if(ch=='I') return 1;
        if(ch=='V') return 5;
        if(ch=='X') return 10;
        if(ch=='L') return 50;
        if(ch=='C') return 100;
        if(ch=='D') return 500;
        return 1000;
    }

    int romanToInt(string s) {

        int ans=0;

        for(int i=0;i<s.size();i++){

            if(i+1<s.size()){

                string pair="";

                pair+=s[i];
                pair+=s[i+1];

                if(pair=="IV"){
                    ans+=4;
                    i++;
                }

                else if(pair=="IX"){
                    ans+=9;
                    i++;
                }

                else if(pair=="XL"){
                    ans+=40;
                    i++;
                }

                else if(pair=="XC"){
                    ans+=90;
                    i++;
                }

                else if(pair=="CD"){
                    ans+=400;
                    i++;
                }

                else if(pair=="CM"){
                    ans+=900;
                    i++;
                }
                else
                    ans+=value(s[i]);
            }
            else
                ans+=value(s[i]);
        }
        return ans;
    }
};
```

---

## Complexity

**Time :** O(n)

**Space :** O(1)

Although this works,

there are many unnecessary conditions.

---

# 2. Optimal Approach (Compare Current and Next Value)

## Intuition

Instead of remembering six subtraction cases,

remember only one rule.

> If current Roman value is smaller than the next value,
>
> subtract it.
>
> Otherwise,
>
> add it.

That's it.

---

## Algorithm

1. Store values of Roman symbols.
2. Traverse string.
3. Compare current value with next value.
4. If current < next

Subtract.

Else

Add.

5. Return answer.

---

# Optimal Code (Fully Commented)

```cpp
class Solution {
public:

    int romanToInt(string s) {

        // Stores value of every Roman symbol.
        unordered_map<char, int> value = {

            {'I',1},
            {'V',5},
            {'X',10},
            {'L',50},
            {'C',100},
            {'D',500},
            {'M',1000}
        };

        // Final integer answer.
        int ans = 0;
        // Traverse every character.
        for(int i = 0; i < s.size(); i++) {

            // If current character is not the last one
            // AND current value is smaller than next value,
            // then it is a subtraction case.
            if(i + 1 < s.size() &&
               value[s[i]] < value[s[i+1]]) {
                ans -= value[s[i]];
            }

            // Otherwise simply add its value.
            else {
                ans += value[s[i]];
            }
        }
        return ans;
    }
};
```

---

## Complexity

**Time :** O(n)

**Space :** O(1)

---

# Understanding Every Line

---

```cpp
unordered_map<char,int> value;
```

Think of this as a dictionary.

```
Roman Symbol

↓

Integer
```

It stores

```text
I → 1

V → 5

X → 10

L → 50

C → 100

D → 500

M → 1000
```

Now whenever we need the value of

```text
X
```

we simply write

```cpp
value['X']
```

Output

```text
10
```

---

```cpp
int ans = 0;
```

Stores our final integer.

Initially

```text
0
```

---

```cpp
for(int i=0;i<s.size();i++)
```

Visit every Roman symbol.

Suppose

```text
MCM
```

Characters visited

```text
M

C

M
```

---

```cpp
if(i+1<s.size())
```

Why?

Because we want to compare with the next character.

If we're already at the last character,

there is no next character.

---

```cpp
value[s[i]]
```

Suppose

```text
s[i]='L'
```

Dictionary lookup

```text
L

↓

50
```

---

```cpp
value[s[i+1]]
```

Suppose

```text
Next

↓

X
```

Dictionary lookup

```text
10
```

---

```cpp
if(value[s[i]]<value[s[i+1]])
```

This is the most important line.

Example

```text
IV
```

Current

```text
I

↓

1
```

Next

```text
V

↓

5
```

Check

```text
1<5
```

True.

Therefore

subtract.

---

```cpp
ans -= value[s[i]];
```

Instead of

```text
+1
```

we do

```text
-1
```

Later

```text
+5
```

Total

```text
4
```

Exactly correct.

---

If current value is larger

Example

```text
VI
```

Current

```text
5
```

Next

```text
1
```

Check

```text
5<1
```

False.

Go to

```cpp
ans += value[s[i]];
```

Add

```text
5
```

Next iteration

Add

```text
1
```

Final

```text
6
```

---

# Complete Dry Run

Input

```text
MCMXCIV
```

Initially

```text
ans = 0
```

---

Character

```text
M
```

Compare

```text
1000

100
```

1000 < 100 ?

No.

Add

```text
ans = 1000
```

---

Character

```text
C
```

Compare

```text
100

1000
```

100 < 1000

Yes.

Subtract

```text
ans = 900
```

---

Character

```text
M
```

Compare

```text
1000

10
```

Add

```text
1900
```

---

Character

```text
X
```

Compare

```text
10

100
```

Subtract

```text
1890
```

---

Character

```text
C
```

Add

```text
1990
```

---

Character

```text
I
```

Compare

```text
1

5
```

Subtract

```text
1989
```

---

Character

```text
V
```

Add

```text
1994
```

Done.

---