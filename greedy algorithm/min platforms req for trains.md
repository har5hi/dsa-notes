# Minimum Number of Platforms Required for a Railway Station (Greedy Algorithm)

> **Problem Source:** GeeksforGeeks (GFG)

---

# 📌 Problem Statement

Given the arrival and departure times of trains at a railway station, find the **minimum number of platforms** required so that **no train has to wait**.

You are given:

- `arr[]` → arrival times of trains
- `dep[]` → departure times of trains

A platform can be reused **only after a train departs**.

Return the minimum number of platforms needed.

---

## Example 1

```text
Input

arr = [900, 940, 950, 1100, 1500, 1800]

dep = [910, 1200, 1120, 1130, 1900, 2000]

Output

3
```

### Explanation

Around **11:00 AM**, three trains are at the station simultaneously:

```text
Train 2 : 9:40  - 12:00
Train 3 : 9:50  - 11:20
Train 4 : 11:00 - 11:30
```

Hence,

```text
Minimum Platforms = 3
```

---

## Example 2

```text
Input

arr = [900, 1100, 1235]

dep = [1000, 1200, 1240]

Output

1
```

---

# 💡 Intuition

We need to know:

> **At any moment, how many trains are present at the station simultaneously?**

That maximum count is exactly the number of platforms required.

Instead of checking every pair of trains,

we process **arrival** and **departure** events in chronological order.

Think of it like this:

- **Arrival** → One more platform is occupied.
- **Departure** → One platform becomes free.

The highest number of occupied platforms during this process is the answer.

---

# ❌ Brute Force Approach

For every train,

compare it with every other train to count overlaps.

Example:

```
Train 1 overlaps Train 2
Train 1 overlaps Train 3
...
```

Take the maximum overlap.

### Time Complexity

```text
O(n²)
```

Too slow for large inputs.

---

# ✅ Optimal Approach (Greedy + Two Pointers)

### Step 1

Sort

- Arrival times
- Departure times

independently.

---

### Step 2

Maintain two pointers:

```text
i → arrivals

j → departures
```

---

### Step 3

If

```text
arrival <= departure
```

A new train arrives before the earliest departure.

Need one more platform.

---

### Step 4

Else

```text
arrival > departure
```

A train departs first.

One platform becomes free.

---

### Step 5

Track the maximum platforms occupied.

---

# 📝 Algorithm

1. Sort arrival array.
2. Sort departure array.
3. Initialize

```text
platforms = 1

answer = 1

i = 1

j = 0
```

4. While both arrays have trains left:

- If

```text
arr[i] <= dep[j]
```

Increase platforms.

Move arrival pointer.

- Else

```text
platforms--
```

Move departure pointer.

5. Update answer.

6. Return answer.

---

# ✅ C++ Code

```cpp
class Solution {
public:
    int findPlatform(vector<int>& arr, vector<int>& dep) {

        sort(arr.begin(), arr.end());
        sort(dep.begin(), dep.end());

        int platforms = 1;
        int answer = 1;

        int i = 1;
        int j = 0;

        while (i < arr.size() && j < dep.size()) {

            if (arr[i] <= dep[j]) {

                platforms++;
                answer = max(answer, platforms);
                i++;
            }
            else {

                platforms--;
                j++;
            }
        }

        return answer;
    }
};
```

---

# ⏱ Time Complexity

Sorting

```text
O(n log n)
```

Two-pointer traversal

```text
O(n)
```

Overall

```text
O(n log n)
```

---

# 📦 Space Complexity

Only pointers and counters are used.

```text
O(1)
```

(ignoring sorting space)

---

# ▶️ Dry Run

### Input

```text
Arrival

[900,940,950,1100,1500,1800]

Departure

[910,1120,1130,1200,1900,2000]
```

Already sorted.

Initially

```text
platforms = 1

answer = 1

i = 1

j = 0
```

---

### Compare

```text
940 <= 910 ?
```

No.

One train departs.

```text
platforms = 0

j++
```

---

### Compare

```text
940 <= 1120
```

Yes.

New arrival.

```text
platforms = 1

i++
```

---

### Compare

```text
950 <= 1120
```

Yes.

```text
platforms = 2
```

---

### Compare

```text
1100 <= 1120
```

Yes.

```text
platforms = 3

answer = 3
```

---

### Compare

```text
1500 <= 1120 ?
```

No.

Train departs.

```text
platforms = 2
```

Continue until all trains are processed.

Final Answer

```text
3
```

---

# 🔍 Line-by-Line Explanation

```cpp
sort(arr.begin(), arr.end());
```

Sort arrival times.

---

```cpp
sort(dep.begin(), dep.end());
```

Sort departure times.

---

```cpp
int platforms = 1;
```

One platform is occupied by the first train.

---

```cpp
int answer = 1;
```

Stores the maximum platforms required.

---

```cpp
int i = 1;
```

Arrival pointer.

---

```cpp
int j = 0;
```

Departure pointer.

---

```cpp
while(i < arr.size() && j < dep.size())
```

Process all arrival and departure events.

---

```cpp
if(arr[i] <= dep[j])
```

A train arrives before the earliest departure.

Need another platform.

---

```cpp
platforms++;
```

Increase occupied platforms.

---

```cpp
answer = max(answer, platforms);
```

Update the maximum platforms required.

---

```cpp
i++;
```

Move to the next arrival.

---

```cpp
else
```

A train departs first.

---

```cpp
platforms--;
```

Free one platform.

---

```cpp
j++;
```

Move to the next departure.

---

```cpp
return answer;
```

Return the minimum number of platforms required.

---

# 🎯 Why Greedy Works?

Instead of assigning platforms to individual trains,

we only care about the **number of trains present simultaneously**.

Sorting arrivals and departures creates a chronological sequence of events.

Every:

- Arrival increases platform usage.
- Departure decreases platform usage.

The maximum number of occupied platforms at any point is exactly the minimum number of platforms needed.

This avoids unnecessary comparisons and guarantees the optimal answer.

---

# 💼 Interview Tips

### Hint 1

Whenever the problem asks:

> Maximum number of overlapping intervals

Think about:

- Sorting events
- Two pointers

---

### Hint 2

Don't actually simulate railway platforms.

Just count how many trains are simultaneously present.

---

### Common Mistakes

❌ Forgetting to sort both arrays.

❌ Using `<` instead of `<=`.

In the GFG version:

```text
Arrival == Departure
```

still requires a new platform because the arriving train cannot use the platform at the exact same time the departing train is leaving.

❌ Comparing every train with every other train (O(n²)).

---

# 🧠 Pattern Recognition

This is a classic **Sweep Line / Event Processing** problem.

### Pattern

- Sort all events.
- Process them in chronological order.
- Increase count on arrival.
- Decrease count on departure.
- Track the maximum active count.

Similar Problems:

- Meeting Rooms II
- LC 253 – Meeting Rooms II
- LC 732 – My Calendar III
- Maximum Overlapping Intervals
- Interval Scheduling Problems

---

# ⚖️ N Meetings vs Minimum Platforms

| Problem | Goal | Sorting |
|---------|------|---------|
| N Meetings | Max non-overlapping meetings | End time |
| Non-overlapping Intervals | Min removals | End time |
| Minimum Platforms | Max overlapping trains | Arrivals & Departures |

---

# 🎯 Visual Understanding

```
Timeline

900      940      950     1100     1120

Train 1
========

Train 2
     ==========================

Train 3
        =================

Train 4
              ======

At 1100

Platforms occupied

Train 2
Train 3
Train 4

Total = 3
```

Hence,

```text
Minimum Platforms = 3
```

---

# ✅ Final Takeaway

- Treat arrivals and departures as chronological events.
- Sort both arrays independently.
- Use two pointers to process events in time order.
- Increment the platform count on every arrival and decrement it on every departure.
- The maximum number of occupied platforms at any moment is the minimum number of platforms required.
- This greedy sweep-line approach runs in **O(n log n)** time with **O(1)** extra space (excluding sorting).