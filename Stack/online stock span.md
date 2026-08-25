# LeetCode 901 - Online Stock Span

---

# Problem Statement

Design an algorithm that collects daily stock prices and returns the **span** of the stock's price for the current day.

The **span** of the stock's price today is defined as:

> The maximum number of **consecutive days (including today)** for which the stock price was **less than or equal to today's price**.

Implement the `StockSpanner` class.

---

## Example

```text
Input

["StockSpanner","next","next","next","next","next","next","next"]

[[],[100],[80],[60],[70],[60],[75],[85]]
```

Output

```text
[null,1,1,1,2,1,4,6]
```

---

### Explanation

| Day | Price | Span |
|-----|------:|-----:|
| 1 | 100 | 1 |
| 2 | 80 | 1 |
| 3 | 60 | 1 |
| 4 | 70 | 2 |
| 5 | 60 | 1 |
| 6 | 75 | 4 |
| 7 | 85 | 6 |

---

# Brute Force Approach

For every new price, move left until you find a price greater than today's price.
Count all consecutive days.

---

## Time Complexity

```
O(n)
```

per query.

For `n` queries,

```
O(n²)
```

---

# Optimal Intuition

Instead of checking previous prices every time, keep only the **useful previous greater prices**.

Use a **Monotonic Decreasing Stack**.

The stack stores prices that can stop the span.

---

# Key Observation

Suppose prices are

```
100 80 60 70
```

When

```
70
```

arrives,

```
60
```

is useless.

Why? Any future price greater than `70` will also be greater than `60`.

So we remove `60`.

---

# What Does the Stack Store?

Store

```
(price, span)
```

instead of only prices.

Example

```
(100,1)

(80,1)

(70,2)
```

This allows us to directly combine spans.

---

# Why Store Span?

Suppose

```
100 80 60 70 75
```

When

```
75
```

arrives,

```
75 > 70
```

Current span becomes

```
1 + 2 = 3
```

Then

```
75 > 60
```

Current span becomes

```
3 + 1 = 4
```

No need to count each day separately.

---

# Optimal Code

```cpp
class StockSpanner {
public:

    stack<pair<int,int>> st;

    StockSpanner() { }

    int next(int price) {

        int span = 1;

        while(!st.empty() && st.top().first <= price){

            span += st.top().second;
            st.pop();
        }
        st.push({price, span});
        return span;
    }
};
```

---

# Why Store `(price, span)`?

Without span,

consider

```
100 80 60 70 75
```

When

```
75
```

arrives,

you would need to count

```
70

60
```

again.

With stored spans,

```
70
```

already knows

```
span = 2
```

So we simply do

```
1 + 2 + 1 = 4
```

---

# Why Monotonic Decreasing Stack?

The stack always stores prices in

```
High

↓

Low
```

Whenever a larger price arrives,

all smaller prices become useless.

Remove them.

---

# Time Complexity

Each price is

- pushed once
- popped once

Amortized

```
O(1)
```

per query.

Overall

```
O(n)
```

for `n` calls.

---

# Space Complexity

Stack

```
O(n)
```

---

# Interview Tips

The interviewer usually expects this observation:

> Every smaller (or equal) price behind the current price can be merged into one span.

This makes each element enter and leave the stack only once.

---