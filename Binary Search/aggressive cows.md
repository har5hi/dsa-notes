# Aggressive Cows

## Problem Statement

You are given:

```cpp
stalls[]
```

representing positions of stalls on a line and an integer:

```cpp
k
```

representing the number of cows.

Place the cows in the stalls such that the **minimum distance between any two cows is maximized**.

Return that maximum possible minimum distance.

---

## Example

```cpp
Input:

stalls = [1,2,4,8,9]
k = 3

Output:

3
```

### Explanation

Place cows at:

```text
1, 4, 8
```

Minimum distance:

```cpp
min(3,4) = 3
```

No arrangement can produce a larger minimum distance.

---

# Brute Force Approach (Linear Search on Answer)

## Observation

The minimum distance between cows can range from:

```cpp
1
```

to

```cpp
max(stalls) - min(stalls)
```

For every distance:

```cpp
dist
```

check if we can place all cows.

Keep increasing distance until placement becomes impossible.

The last valid distance is the answer.

---

# Helper Function

Check whether we can place:

```cpp
k cows
```

such that minimum distance is at least:

```cpp
dist
```

---

## Greedy Placement

Place first cow at:

```cpp
stalls[0]
```

Then place the next cow at the first stall satisfying:

```cpp
stalls[i] - lastPlaced >= dist
```

---

## Why Greedy Works?

To maximize the chance of placing remaining cows, always place a cow as early as possible.
This leaves maximum space for future cows.

---

# Brute Force Code

```cpp
class Solution {
public:

    bool canPlace(vector<int>& stalls,
                  int dist,
                  int cows) {

        int cntCows = 1;
        int last = stalls[0];

        for(int i = 1; i < stalls.size(); i++) {

            if(stalls[i] - last >= dist) {

                cntCows++;
                last = stalls[i];
            }
        }

        return cntCows >= cows;
    }

    int aggressiveCows(vector<int>& stalls,
                       int cows) {

        sort(stalls.begin(), stalls.end());

        int limit =
            stalls.back() - stalls.front();

        for(int dist = 1;
            dist <= limit;
            dist++) {

            if(!canPlace(stalls,
                         dist,
                         cows)) {

                return dist - 1;
            }
        }

        return limit;
    }
};
```

---

# Complexity Analysis

### Sorting

```cpp
O(n log n)
```

### Linear Search

```cpp
O(maxDistance)
```

### Placement Check

```cpp
O(n)
```

### Total

```cpp
O(n log n + n × maxDistance)
```
Too slow for large constraints.

---

# Optimal Approach (Binary Search on Answer)

## Key Observation

Suppose:

```cpp
distance = 3
```

works.

Then:

```cpp
1
2
3
```

will also work.

---

Suppose:

```cpp
distance = 4
```

does not work.

Then:

```cpp
5
6
7
```

will never work.

---

This forms a monotonic pattern:

```text
Distance

1   2   3   4   5   6
✓   ✓   ✓   ✗   ✗   ✗
```

We need:

```text
Largest Valid Distance
```

Hence Binary Search.

---

# Search Space

Minimum possible distance:

```cpp
low = 1
```

Maximum possible distance:

```cpp
high = stalls[n-1] - stalls[0]
```

---

# Binary Search Logic

### If distance works

```cpp
canPlace(mid) == true
```

Try larger distance.

```cpp
low = mid + 1
```

---

### If distance fails

```cpp
canPlace(mid) == false
```

Need smaller distance.

```cpp
high = mid - 1
```

---

# Why Are We Searching Right When Valid?

We want:

```text
Maximum Possible Minimum Distance
```

Not just any valid distance.

So whenever a distance works:

```cpp
search larger values
```

---

# Optimal Code

```cpp
class Solution {
public:

    bool canPlace(vector<int>& stalls,
                  int dist,
                  int cows) {

        int cntCows = 1;
        int last = stalls[0];

        for(int i = 1; i < stalls.size(); i++) {

            if(stalls[i] - last >= dist) {

                cntCows++;
                last = stalls[i];
            }
        }

        return cntCows >= cows;
    }

    int aggressiveCows(vector<int>& stalls,
                       int cows) {

        sort(stalls.begin(), stalls.end());

        int low = 1;

        int high =
            stalls.back() - stalls.front();

        while(low <= high) {

            int mid =
                low + (high - low) / 2;

            if(canPlace(stalls,
                        mid,
                        cows)) {

                low = mid + 1;
            }
            else {

                high = mid - 1;
            }
        }

        return high;
    }
};
```

---

# Complexity Analysis

### Sorting

```cpp
O(n log n)
```

### Binary Search

```cpp
O(log(maxDistance))
```

### Each Placement Check

```cpp
O(n)
```

### Total

| Complexity | Value |
|------------|--------|
| Time | O(n log n + n log(maxDistance)) |
| Space | O(1) |