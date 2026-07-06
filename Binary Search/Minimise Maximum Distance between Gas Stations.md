# Minimise Maximum Distance Between Gas Stations

## Problem Statement

You are given:

```cpp
arr[]
```

where:

```cpp
arr[i]
```

represents the position of the `i-th` gas station on a straight road.

You are also given:

```cpp
k
```

which represents the number of additional gas stations that can be placed.

Place exactly `k` new gas stations such that the **maximum distance between adjacent gas stations is minimized**.

Return the minimum possible maximum distance.

---

## Example

```cpp
arr = [1,2,3,4,5]
k = 4
```

Output:

```cpp
0.5
```

---

## Why?

Original gaps:

```text
1 2 3 4 5
```

Distances:

```text
1 1 1 1
```

Adding 4 stations:

```text
1 1.5 2 2.5 3 3.5 4 4.5 5
```

Maximum distance:

```cpp
0.5
```

---

# Brute Force Approach (Greedy)

## Idea

Always split the largest segment.

Example:

```cpp
arr = [1,7]
k = 2
```

Gap:

```cpp
6
```

---

Add one station:

```text
1 --- 4 --- 7
```

Largest gap:

```cpp
3
```

---

Add another station:

```text
1 -- 3 -- 5 -- 7
```

Largest gap:

```cpp
2
```

---

Continue choosing the segment having the largest current distance.

---

# Brute Force Code

```cpp
double minimiseMaxDistance(vector<int> &arr, int k) {

    int n = arr.size();

    vector<int> howMany(n - 1, 0);

    for(int gasStations = 1; gasStations <= k; gasStations++) {

        long double maxSection = -1;
        int maxIndex = -1;

        for(int i = 0; i < n - 1; i++) {

            long double diff = arr[i + 1] - arr[i];

            long double sectionLength = diff / (long double) (howMany[i] + 1);

            if(sectionLength > maxSection) {
                maxSection = sectionLength;
                maxIndex = i;
            }
        }
        howMany[maxIndex]++;
    }

    long double ans = -1;

    for(int i = 0; i < n - 1; i++) {

        long double diff = arr[i + 1] - arr[i];

        long double sectionLength = diff / (long double)(howMany[i] + 1);

        ans = max(ans, sectionLength);
    }
    return ans;
}
```

---

# Complexity Analysis

Outer Loop:

```cpp
k times
```

Inner Loop:

```cpp
n times
```

Total:

```cpp
O(n × k)
```

Too slow.

---

# Better Approach (Priority Queue)

## Observation

Instead of searching for the largest section every time,

store them in a max heap.

---

For every segment:

```cpp
gap = arr[i+1] - arr[i]
```

Store:

```cpp
(currentLargestSection, index)
```

in a priority queue.

---

Each time:

1. Pick largest segment.
2. Add a station.
3. Recompute its section size.
4. Push back into heap.

---

# Code

```cpp
double minimiseMaxDistance(vector<int> &arr, int k) {

    int n = arr.size();

    vector<int> howMany(n - 1, 0);

    priority_queue<pair<long double,int>> pq;

    for(int i = 0; i < n - 1; i++) {

        pq.push({
            arr[i + 1] - arr[i], i
        });
    }

    for(int gasStations = 1; gasStations <= k; gasStations++) {

        auto top = pq.top();
        pq.pop();

        int secIndex = top.second;

        howMany[secIndex]++;

        long double initialDiff = arr[secIndex + 1] - arr[secIndex];

        long double newSection = initialDiff / (long double) (howMany[secIndex] + 1);

        pq.push({
            newSection,
            secIndex
        });
    }
    return pq.top().first;
}
```

---

# Complexity Analysis

Priority Queue:

```cpp
O(log n)
```

performed:

```cpp
k times
```

Total:

```cpp
O(k log n)
```

Better but still large when:

```cpp
k = 10^6
```

---

# Optimal Approach (Binary Search on Answer)

## Key Observation

Suppose maximum distance:

```cpp
D = 3
```

is possible.

Then:

```cpp
4
5
6
```

are also possible.

---

Suppose:

```cpp
D = 1
```

is not possible.

Then:

```cpp
0.9
0.8
0.7
```

will never be possible.

---

Monotonic pattern:

```text
Distance

0.5 1.0 1.5 2.0 2.5 3.0
✗   ✗   ✗   ✓   ✓   ✓
```

Need:

```text
Smallest Valid Distance
```

Hence Binary Search.

---

# Key Formula Derivation

Consider a gap:

```cpp
gap = 9
```

Maximum allowed distance:

```cpp
dist = 3
```

---

Need sections:

```text
3 | 3 | 3
```

Number of sections:

```cpp
9 / 3 = 3
```

Stations required:

```cpp
3 - 1 = 2
```

---

Formula:

```cpp
stationsNeeded = gap / dist
```

But if:

```cpp
gap % dist == 0
```

one station gets counted extra.

So:

```cpp
stationsNeeded--;
```

---

Implementation:

```cpp
int numberInBetween = ((arr[i+1]-arr[i]) / dist);
```

If exactly divisible:

```cpp
numberInBetween--;
```

---

# Helper Function

For a distance:

```cpp
dist
```

count stations required.

---

## Code

```cpp
int numberOfGasStationsRequired(long double dist, vector<int> &arr) {

    int count = 0;

    for(int i = 1; i < arr.size(); i++) {

        long double gap = arr[i] - arr[i - 1];

        int numberInBetween = gap / dist;

        if((gap/dist) == (int)(gap/dist)) {
            numberInBetween--;
        }
        count += numberInBetween;
    }
    return count;
}
```

---

# Binary Search

### Search Space

Minimum:

```cpp
0
```

Maximum:

```cpp
largest gap
```

---

### If

```cpp
requiredStations > k
```

Need larger distance.

```cpp
low = mid
```

---

### Else

```cpp
requiredStations <= k
```

Distance is possible.

Try smaller.

```cpp
high = mid
```

---

# Optimal Code

```cpp
class Solution {
public:

    int numberOfGasStationsRequired(long double dist, vector<int> &arr) {

        int count = 0;

        for(int i = 1; i < arr.size(); i++) {

            long double gap = arr[i] - arr[i - 1];

            int numberInBetween = gap / dist;

            if((gap / dist) == (int)(gap / dist)) {
                numberInBetween--;
            }
            count += numberInBetween;
        }
        return count;
    }

    double minimiseMaxDistance(vector<int> &arr, int k) {

        long double low = 0;
        long double high = 0;

        for(int i = 0; i < arr.size() - 1; i++) {

            high = max(high, (long double) (arr[i+1]-arr[i]));
        }

        long double diff = 1e-6;

        while(high - low > diff) {

            long double mid = (low + high) / 2.0;

            int required = numberOfGasStationsRequired(mid, arr);

            if(required > k) {
                low = mid;
            }
            else {
                high = mid;
            }
        }
        return high;
    }
};
```

---

# Dry Run

```cpp
arr = [1,7]
k = 2
```

---

### Gap

```cpp
6
```

Search:

```cpp
0 → 6
```

---

### Mid = 3

Required stations:

```cpp
6/3 = 2
```

Need:

```cpp
1 station
```

Valid.

Search smaller.

---

### Mid = 2

Required stations:

```cpp
6/2 = 3
```

Need:

```cpp
2 stations
```

Valid.

Search smaller.

---

### Mid = 1.5

Required:

```cpp
3 stations
```

Not possible.

Search right.

---

Eventually converges to:

```cpp
2.0
```

---

# Complexity Analysis

Let:

```cpp
n = number of stations
```

Binary Search iterations:

```cpp
≈ 60
```

(for precision 1e-6)

Each check:

```cpp
O(n)
```

Total:

| Complexity | Value |
|------------|--------|
| Time | O(n × log(range/precision)) |
| Space | O(1) |

---