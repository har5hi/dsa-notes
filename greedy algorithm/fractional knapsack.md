# Fractional Knapsack Problem (Greedy Algorithm)

## 📌 Problem Statement

You are given:

- A knapsack with capacity **W**.
- `N` items.
- Each item has:
  - **Value**
  - **Weight**

Unlike the **0/1 Knapsack**, here you are allowed to take **fractions of an item**.

Your goal is to maximize the **total value** stored in the knapsack.

---

### Example

```text
Capacity = 50

Items

Value   Weight
60      10
100     20
120     30
```

Output

```text
240
```

Explanation

Take:

```
Item 1 completely

Weight = 10
Value = 60
```

Take:

```
Item 2 completely

Weight = 20
Value = 100
```

Remaining capacity

```
50 - 30 = 20
```

Take

```
20/30 of Item 3
```

Value obtained

```
120 × (20/30) = 80
```

Total value

```
60 + 100 + 80 = 240
```

---

# 💡 Intuition

If we can take fractions, then we should always choose the item that gives the **maximum value per unit weight**.

Example

| Item | Value | Weight | Value/Weight |
|------|------:|-------:|-------------:|
| A | 60 | 10 | 6 |
| B | 100 | 20 | 5 |
| C | 120 | 30 | 4 |

Item A gives the highest profit for every unit of weight.

So we should always fill the knapsack with the **highest value/weight ratio** first.

This is the greedy choice.

---

# Optimal Approach (Greedy)

### Step 1

Compute

```
Value / Weight
```

for every item.

---

### Step 2

Sort items in **descending order** of

```
Value / Weight
```

---

### Step 3

Traverse the sorted list.

If the whole item fits

```
Take it completely.
```

Otherwise

```
Take only the required fraction.
```

Stop because the bag becomes full.

---

# C++ Code 

```cpp
class Solution {
public:

    static bool cmp(Item a, Item b) {
        double r1 = (double)a.value / a.weight;
        double r2 = (double)b.value / b.weight;
        return r1 > r2;
    }

    double fractionalKnapsack(int W, Item arr[], int n) {
        sort(arr, arr + n, cmp);
        double ans = 0.0;

        for (int i = 0; i < n; i++) {
            if (arr[i].weight <= W) {
                ans += arr[i].value;
                W -= arr[i].weight;
            } else {
                ans += ((double)arr[i].value / arr[i].weight) * W;
                break;
            }
        }
        return ans;
    }
};
```

---

# ⏱ Time Complexity

Sorting

```
O(n log n)
```

Traversal

```
O(n)
```

Overall

```
O(n log n)
```

---

# 📦 Space Complexity

```
O(1)
```

(ignoring sorting space)

---

# 🔍 Line-by-Line Explanation

```cpp
static bool cmp(Item a, Item b)
```

Comparator used for sorting.

---

```cpp
double r1 = (double)a.value / a.weight;
```

Compute value per unit weight of item A.

---

```cpp
return r1 > r2;
```

Sort in descending order.

Highest ratio comes first.

---

```cpp
sort(arr, arr + n, cmp);
```

Sort all items by profit density.

---

# 🎯 Why Greedy Works?

Suppose two items:

| Item | Value | Weight | Ratio |
|------|------:|--------:|-------:|
|A|100|20|5|
|B|60|15|4|

If you have limited capacity, taking Item A first always gives more value per unit weight than taking Item B.

Since fractions are allowed, taking the highest value/weight ratio first can never reduce the optimal answer.

This is why the greedy strategy is always correct.

> **Note:** This approach works only because items can be divided. If fractions were not allowed (0/1 Knapsack), this greedy strategy does **not** guarantee the optimal answer.

---

# ⚖️ Fractional Knapsack vs 0/1 Knapsack

| Feature | Fractional Knapsack | 0/1 Knapsack |
|---------|----------------------|--------------|
|Can take fractions?|✅ Yes|❌ No|
|Approach|Greedy|Dynamic Programming|
|Sort by ratio?|✅ Yes|❌ No|
|Time Complexity|O(n log n)|O(n × W)|
|Optimal with Greedy?|✅ Yes|❌ No|