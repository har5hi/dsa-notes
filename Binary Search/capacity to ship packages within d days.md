# 1011. Capacity To Ship Packages Within D Days

## Problem Statement

You are given an array:

```cpp
weights[]
```

where:

```cpp
weights[i]
```

represents the weight of the `i-th` package.

Packages must be shipped in the given order.

You are also given:

```cpp
days
```

Return the **least ship capacity** required to ship all packages within the given number of days.

---

## Example 1

```cpp
Input:

weights = [1,2,3,4,5,6,7,8,9,10]
days = 5

Output:

15
```

---

### Explanation

Capacity = 15

```text
Day 1 : 1 2 3 4 5 = 15

Day 2 : 6 7 = 13

Day 3 : 8

Day 4 : 9

Day 5 : 10
```

All packages shipped within:

```cpp
5 days
```

---

# Brute Force Approach

## Idea

Try every capacity from:

```cpp
max(weights)
```

to

```cpp
sum(weights)
```

For each capacity:

- Simulate shipping.
- Count days required.
- Return first capacity that works.

---

## Complexity

Let:

```cpp
n = weights.size()
```

and

```cpp
S = sum(weights)
```

```cpp
Time = O((S - maxWeight) × n)
```

Too slow.

---

# Optimal Approach (Binary Search on Answer)

## Key Observation

Suppose:

```cpp
Capacity = 10
```

and it ships everything in:

```cpp
≤ days
```

Then:

```cpp
Capacity = 11
Capacity = 12
Capacity = 13
```

will also work.

---

Similarly:

If capacity 10 cannot ship in time,

then:

```cpp
1,2,3,...,9
```

will never work.

---

This forms a monotonic pattern:

```text
Capacity

1   2   3   4   5   6   7
✗   ✗   ✗   ✗   ✓   ✓   ✓
```

Need:

```text
First Valid Capacity
```

Hence Binary Search.

---

# Search Space

## Minimum Capacity

Must be at least:

```cpp
max(weights)
```

Because every package must fit into the ship.

```cpp
low = max(weights)
```

---

## Maximum Capacity

Ship everything in one day.

```cpp
high = sum(weights)
```

---

# Helper Function

For a given capacity:

```cpp
capacity
```

calculate how many days are needed.

---

### Logic

Keep loading packages. If adding the next package exceeds capacity:

```cpp
days++
```

Start a new day.

---

# Example

```cpp
weights = [1,2,3,4,5]

capacity = 5
```

---

### Day 1

```text
1 + 2 = 3
```

Cannot add 3.

```text
Days = 1
```

---

### Day 2

```text
3
```

Cannot add 4.

```text
Days = 2
```

---

### Day 3

```text
4
```

Cannot add 5.

```text
Days = 3
```

---

### Day 4

```text
5
```

Total:

```cpp
4 days
```

---

# Binary Search Logic

### If

```cpp
requiredDays <= days
```

Capacity works.

Try smaller capacity.

```cpp
high = mid - 1
```

---

### If

```cpp
requiredDays > days
```

Capacity too small.

Need larger capacity.

```cpp
low = mid + 1
```

---

# Optimal Code

```cpp
class Solution {
public:

    int findDays(vector<int>& weights,
                 int capacity) {

        int days = 1;
        int load = 0;

        for(int weight : weights) {

            if(load + weight > capacity) {

                days++;
                load = weight;
            }
            else {

                load += weight;
            }
        }

        return days;
    }

    int shipWithinDays(vector<int>& weights,
                       int days) {

        int low =
            *max_element(weights.begin(),
                         weights.end());

        int high =
            accumulate(weights.begin(),
                       weights.end(),
                       0);

        while(low <= high) {

            int mid =
                low + (high - low) / 2;

            int requiredDays =
                findDays(weights, mid);

            if(requiredDays <= days) {

                high = mid - 1;
            }
            else {

                low = mid + 1;
            }
        }

        return low;
    }
};
```

---

# Complexity Analysis

Let:

```cpp
n = weights.size()
```

Let:

```cpp
S = sum(weights)
```

---

### Binary Search

```cpp
O(log S)
```

### Each Check

```cpp
O(n)
```

### Total

| Complexity | Value |
|------------|--------|
| Time | O(n × log(sum(weights))) |
| Space | O(1) |

---