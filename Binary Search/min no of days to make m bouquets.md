# 1482. Minimum Number of Days to Make m Bouquets

## Problem Statement

You are given:

```cpp
bloomDay[i]
```

where `bloomDay[i]` represents the day on which the `i-th` flower blooms.

To make **one bouquet**, you need:

```cpp
k adjacent flowers
```

You need to make:

```cpp
m bouquets
```

Return the **minimum number of days** required to make `m` bouquets.

If it is impossible, return:

```cpp
-1
```

---

## Example 1

```cpp
Input:
bloomDay = [1,10,3,10,2]
m = 3
k = 1

Output:
3
```

Explanation:

```text
Day 1 → [1]
Day 2 → [1,2]
Day 3 → [1,2,3]
```

We can make 3 bouquets on day 3.

---

# Brute Force Approach

## Idea

Try every day from:

```cpp
min(bloomDay)
```

to

```cpp
max(bloomDay)
```

For each day:

- Count bloomed flowers.
- Check if `m` bouquets can be formed.

Return first valid day.

---

## Complexity

Let:

```cpp
n = bloomDay.size()
D = maxBloomDay
```

```cpp
Time = O(n × D)
```

Too slow.

---

# Optimal Approach (Binary Search on Answer)

## Key Observation

Suppose:

```cpp
Day = 5
```

and we can make `m` bouquets.

Then:

```cpp
Day = 6
Day = 7
Day = 8
```

will also work because more flowers bloom.

---

Similarly:

If day 5 does NOT work,

```cpp
Day = 4
Day = 3
```

will never work.

---

This creates a monotonic pattern:

```text
Days

1  2  3  4  5  6  7
✗  ✗  ✗  ✗  ✓  ✓  ✓
```

We need:

```text
First Valid Day
```

Hence Binary Search.

---

# Search Space

Minimum possible answer:

```cpp
min(bloomDay)
```

Maximum possible answer:

```cpp
max(bloomDay)
```

```cpp
low = min(bloomDay)
high = max(bloomDay)
```

---

# Important Edge Case

Before Binary Search:

Check whether enough flowers exist.

Need:

```cpp
m * k flowers
```

If:

```cpp
m * k > n
```

Impossible.

Return:

```cpp
-1
```

---

# Helper Function

For a given day:

```cpp
day
```

Count how many bouquets can be formed.

---

### Flower Available

```cpp
bloomDay[i] <= day
```

Flower has bloomed.

```cpp
count++
```

---

### Flower Not Available

```cpp
bloomDay[i] > day
```

Current adjacent sequence breaks.

Create bouquets from collected flowers:

```cpp
bouquets += count / k
```

Reset:

```cpp
count = 0
```

---

### After Loop

One last segment may remain.

```cpp
bouquets += count / k
```

---

If:

```cpp
bouquets >= m
```

Day is valid.

---

# Why Count / k Works?

Example:

```text
Bloomed segment:

1 1 1 1 1 1
```

Length:

```cpp
6
```

Need:

```cpp
k = 2
```

Possible bouquets:

```cpp
6 / 2 = 3
```

---

Example:

```text
1 1 1 1 1
```

Need:

```cpp
k = 2
```

Bouquets:

```cpp
5 / 2 = 2
```

---

# Optimal Code

```cpp
class Solution {
public:

    bool canMake(vector<int>& bloomDay,
                 int day,
                 int m,
                 int k) {

        int count = 0;
        int bouquets = 0;

        for(int bloom : bloomDay) {

            if(bloom <= day) {
                count++;
            }
            else {

                bouquets += count / k;
                count = 0;
            }
        }

        bouquets += count / k;

        return bouquets >= m;
    }

    int minDays(vector<int>& bloomDay,
                int m,
                int k) {

        long long flowersNeeded =
            1LL * m * k;

        if(flowersNeeded > bloomDay.size())
            return -1;

        int low =
            *min_element(bloomDay.begin(),
                         bloomDay.end());

        int high =
            *max_element(bloomDay.begin(),
                         bloomDay.end());

        while(low <= high) {

            int mid =
                low + (high - low) / 2;

            if(canMake(bloomDay,
                       mid,
                       m,
                       k)) {

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
n = bloomDay.size()
```

Let:

```cpp
D = maxBloomDay - minBloomDay
```

---

### Binary Search

```cpp
O(log D)
```

### Each Check

```cpp
O(n)
```

### Total

| Complexity | Value |
|------------|--------|
| Time | O(n × log(maxBloomDay - minBloomDay)) |
| Space | O(1) |