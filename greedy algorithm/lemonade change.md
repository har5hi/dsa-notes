# LeetCode 860 - Lemonade Change

## 📌 Problem Statement

At a lemonade stand, each lemonade costs **$5**.

Customers pay with one of the following bills:

- `$5`
- `$10`
- `$20`

You must provide the correct change for every customer **in the order they arrive**.

Initially, you have **no money**.

Return:

- `true` if you can provide correct change to every customer.
- `false` otherwise.

---

### Example 1

```text
Input:
bills = [5,5,5,10,20]

Output:
true
```

Explanation:

```
Customer 1 pays $5
No change needed

Customer 2 pays $5
No change needed

Customer 3 pays $5
No change needed

Customer 4 pays $10
Give one $5 back

Customer 5 pays $20
Give one $10 and one $5
```

---

### Example 2

```text
Input:
bills = [5,5,10,10,20]

Output:
false
```

Explanation:

```
Last customer needs $15 change.

We only have two $10 bills.
No $5 bill is available.
```

---

# 💡 Intuition

Since every lemonade costs **$5**, there are only three possible cases:

- Customer pays **$5** → No change required.
- Customer pays **$10** → Need to give back one **$5**.
- Customer pays **$20** → Need to give back **$15**.

For `$15` change, there are two possibilities:

```
$10 + $5
```

or

```
$5 + $5 + $5
```

Which one should we prefer?

Always use:

```
$10 + $5
```

because **$5 bills are more valuable** for making future change.

This is a classic **Greedy** decision.

---

# Optimal Approach (Greedy)

Maintain only two counters:

```
five = number of $5 bills
ten  = number of $10 bills
```

Process customers one by one.

### Case 1

Customer pays `$5`

```
five++
```

---

### Case 2

Customer pays `$10`

Need one `$5`.

If no `$5` exists

```
return false
```

Otherwise

```
five--
ten++
```

---

### Case 3

Customer pays `$20`

Need `$15`.

Always try

```
$10 + $5
```

first.

If not possible, use

```
$5 + $5 + $5
```

If neither works

```
return false
```

---

# C++ Code

```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {

        int five = 0;
        int ten = 0;

        for (int bill : bills) {

            if (bill == 5) {
                five++;
            }

            else if (bill == 10) {
                if (five == 0)
                    return false;
                five--;
                ten++;
            }

            else {
                if (ten > 0 && five > 0) {
                    ten--;
                    five--;
                }
                else if (five >= 3) {
                    five -= 3;
                }
                else {
                    return false;
                }
            }
        }
        return true;
    }
};
```

---

# ⏱ Time Complexity

Single traversal.

```
O(n)
```

---

# 📦 Space Complexity

Only two variables.

```
O(1)
```

---

# 🎯 Why Greedy Works?

Suppose you need to return **$15**.

Available bills:

```
five = 4
ten = 1
```

Two options:

Option 1

```
10 + 5
```

Remaining

```
five = 3
ten = 0
```

Option 2

```
5 + 5 + 5
```

Remaining

```
five = 1
ten = 1
```

Keeping the `$10` may seem good, but **$5 bills are the most important** because:

- Every `$10` customer requires a `$5` bill as change.
- A `$20` customer can also require `$5` bills.

Thus, preserving more `$5` bills is beneficial, and using `$10 + $5` whenever possible is the optimal greedy choice.

---

# 💼 Interview Tips

### Hint 1

Notice that the number of bill denominations is very small.

You don't need a map or array—just keep track of `$5` and `$10` bills.

---

### Hint 2

Whenever a greedy choice preserves the most flexible resource (`$5` bills), it's likely to be optimal.

---

# 🧠 Pattern Recognition

This is a classic **Greedy Simulation** problem.

### Pattern

- Process events sequentially.
- Maintain only the essential state.
- Make the locally optimal decision at each step.
- Never revisit previous decisions.