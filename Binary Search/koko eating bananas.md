# 875. Koko Eating Bananas

## Problem Statement

Koko loves bananas. There are `n` piles of bananas where:

```cpp
piles[i]
```

represents the number of bananas in the `i-th` pile.

Koko can decide an eating speed:

```cpp
k bananas/hour
```

Every hour:

- he chooses one pile.
- Eats up to `k` bananas from that pile.
- If the pile has fewer than `k` bananas, he eats all of them.

Given:

```cpp
piles[]
h
```

Return the **minimum integer eating speed `k`** such that Koko can finish all bananas within `h` hours.

---

# Brute Force Approach

## Idea

Try every eating speed:

```cpp
k = 1 to max(piles)
```

For each speed:

- Calculate total hours needed.
- Return the first speed that works.

---

## Brute Force Code

```cpp
class Solution {
public:

    long long totalHours(vector<int>& piles, int k) {

        long long hours = 0;

        for(int pile : piles) {
            hours += ceil((double)pile / k);
        }

        return hours;
    }

    int minEatingSpeed(vector<int>& piles, int h) {

        int maxi = *max_element(piles.begin(), piles.end());

        for(int k = 1; k <= maxi; k++) {

            if(totalHours(piles, k) <= h)
                return k;
        }

        return -1;
    }
};
```

---

## Complexity Analysis

| Complexity | Value |
|------------|--------|
| Time | O(maxPile × n) |
| Space | O(1) |

Too slow for large constraints.

---

# Optimal Approach (Binary Search on Answer)

## Key Observation

Suppose:

```cpp
piles = [3,6,7,11]
h = 8
```

Check different speeds:

```text
Speed = 1 → 27 hours
Speed = 2 → 15 hours
Speed = 3 → 10 hours
Speed = 4 → 8 hours
Speed = 5 → 8 hours
Speed = 6 → 6 hours
```

Notice:

```text
Speed ↑
Hours ↓
```

If a speed works:

```cpp
k = 5
```

Then:

```cpp
6,7,8...
```

will also work.

This forms a monotonic pattern.

Hence Binary Search can be applied.

---

# Search Space

Minimum possible speed:

```cpp
1
```

Maximum possible speed:

```cpp
max(piles)
```

Because eating faster than the largest pile is useless.

```cpp
low = 1
high = max(piles)
```

---

# Helper Function

Calculate total hours needed for a given speed.

Formula:

```cpp
ceil(pile / speed)
```

Instead of using floating point:

```cpp
(pile + speed - 1) / speed
```

Example:

```cpp
pile = 7
speed = 3

(7 + 3 - 1)/3
= 9/3
= 3
```

---

# Binary Search Logic

### If

```cpp
totalHours <= h
```

Current speed works.

Try smaller speed.

```cpp
high = mid - 1
```

---

### If

```cpp
totalHours > h
```

Current speed too slow.

Need larger speed.

```cpp
low = mid + 1
```

---

# Optimal Code

```cpp
class Solution {
public:

    long long calculateHours(vector<int>& piles, int speed) {

        long long totalHours = 0;

        for(int pile : piles) {
            totalHours += (pile + speed - 1) / speed;
        }

        return totalHours;
    }

    int minEatingSpeed(vector<int>& piles, int h) {

        int low = 1;
        int high = *max_element(piles.begin(), piles.end());

        while(low <= high) {

            int mid = low + (high - low) / 2;

            long long hours =
                calculateHours(piles, mid);

            if(hours <= h) {
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

# Why Return `low`?

Binary Search ends when:

```cpp
low > high
```

At that point:

```cpp
high = last invalid speed
```

```cpp
low = first valid speed
```

Since we need:

```cpp
minimum valid speed
```

Return:

```cpp
low
```

---

# Complexity Analysis

Let:

```cpp
n = piles.size()
m = max(piles)
```

### Binary Search

```cpp
O(log m)
```

### Each Check

```cpp
O(n)
```

### Total

| Complexity | Value |
|------------|--------|
| Time | O(n × log(maxPile)) |
| Space | O(1) |

---

# Why This Is Binary Search on Answer

We are not searching an array. We are searching for the answer:

```cpp
speed k
```

inside a range:

```cpp
1 → maxPile
```

and checking:

```cpp
Can Koko finish within h hours?
```

This validity check is monotonic. Hence Binary Search on Answer.