# 121. Best Time to Buy and Sell Stock

## Problem Statement

You are given an array `prices` where `prices[i]` is the price of a stock on the `i-th` day.

Choose a day to buy one stock and choose a different future day to sell that stock.

Return the maximum profit you can achieve. If no profit is possible, return `0`.

---

## Brute Force Approach

### Idea

Try every possible pair of days:

- Buy on day `i`
- Sell on day `j` where `j > i`
- Calculate profit `prices[j] - prices[i]`
- Keep track of the maximum profit

### Complexity

```text
Time Complexity  : O(n²)
Space Complexity : O(1)
```

---

## Optimal Approach (One Pass)

### Observation

To maximize profit:

```text
Profit = Selling Price - Buying Price
```

For every day:

- Keep track of the minimum price seen so far.
- Treat the current price as the selling price.
- Calculate profit if sold today.
- Update the maximum profit.

---

## Algorithm

1. Initialize:

```cpp
minPrice = prices[0]
maxProfit = 0
```

2. Traverse the array from left to right.

3. For each price:

   - Update minimum buying price seen so far.
   - Calculate profit if sold today.
   - Update maximum profit.

4. Return `maxProfit`.

---

## Code

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();

        int minPrice = prices[0];
        int maxProfit = 0;

        for(int i = 1; i < n; i++) {
            minPrice = min(minPrice, prices[i]);
            maxProfit = max(maxProfit, prices[i] - minPrice);
        }

        return maxProfit;
    }
};
```

---

## Dry Run

### Input

```cpp
prices = [7,1,5,3,6,4]
```

| i | prices[i] | minPrice | Current Profit | maxProfit |
|---|-----------|-----------|---------------|-----------|
| 0 | 7 | 7 | - | 0 |
| 1 | 1 | 1 | 0 | 0 |
| 2 | 5 | 1 | 4 | 4 |
| 3 | 3 | 1 | 2 | 4 |
| 4 | 6 | 1 | 5 | 5 |
| 5 | 4 | 1 | 3 | 5 |

### Result

```cpp
maxProfit = 5
```

---

## Why This Works

At every index:

- `minPrice` stores the best buying opportunity seen so far.
- `prices[i] - minPrice` gives the profit if we sell today.
- We continuously keep the best profit found.

This ensures:

- One traversal only.
- No need to check all pairs.

---