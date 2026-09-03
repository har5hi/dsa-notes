# N Meetings in One Room (Greedy Algorithm)

---

# 📌 Problem Statement

There is one meeting room.

You are given two arrays:

- `start[]` → starting time of each meeting.
- `end[]` → ending time of each meeting.

Only **one meeting** can be held in the room at a time.

Your task is to find the **maximum number of meetings** that can be performed in the meeting room.

> **Note:** A meeting can be scheduled only if its start time is **strictly greater** than the end time of the previously selected meeting.

---

## Example 1

```text
Input

start = [1,3,0,5,8,5]
end   = [2,4,6,7,9,9]

Output

4
```

### Explanation

Selected meetings:

```
(1,2)
(3,4)
(5,7)
(8,9)
```

Total meetings = **4**

---

## Example 2

```text
Input

start = [10,12,20]
end   = [20,25,30]

Output

1
```

---

# 💡 Intuition

We want to perform the **maximum number of meetings**.

Suppose we choose a meeting that ends very late.

```
Meeting A

1 ------ 10
```

Now many meetings occurring between 2 and 9 cannot be scheduled.

Instead, if we choose the meeting that finishes earliest,

```
1 --2
```

the room becomes available sooner, leaving more time for future meetings.

So the greedy choice is:

> **Always select the meeting that finishes first.**

---

# Optimal Approach (Greedy)

### Step 1

Store every meeting as

```
(start, end)
```

---

### Step 2

Sort meetings according to

```
Ending Time
```

in ascending order.

---

### Step 3

Select the first meeting.

---

### Step 4

Traverse remaining meetings.

If

```
current.start > previousEnd
```

select the meeting.

Update

```
previousEnd
```

---

### Step 5

Return the total count.

---

# C++ Code

```cpp
class Solution {
public:

    static bool cmp(pair<int,int> a, pair<int,int> b) {
        return a.second < b.second;
    }

    int maxMeetings(vector<int>& start, vector<int>& end) {
        int n = start.size();

        vector<pair<int,int>> meetings;

        for (int i = 0; i < n; i++) {
            meetings.push_back({start[i], end[i]});
        }

        sort(meetings.begin(), meetings.end(), cmp);

        int count = 1;
        int prevEnd = meetings[0].second;

        for (int i = 1; i < n; i++) {
            if (meetings[i].first > prevEnd) {
                count++;
                prevEnd = meetings[i].second;
            }
        }
        return count;
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

Vector of meetings

```
O(n)
```

---

# 🔍 Line-by-Line Explanation

```cpp
vector<pair<int,int>> meetings;
```

Store every meeting as

```
(start, end)
```

---

```cpp
meetings.push_back({start[i], end[i]});
```

Insert every meeting.

---

```cpp
sort(meetings.begin(), meetings.end(), cmp);
```

Sort according to ending time.

---

```cpp
int count = 1;
```

Always select the earliest-ending meeting first.

---

```cpp
int prevEnd = meetings[0].second;
```

Store the ending time of the selected meeting.

---

```cpp
for(int i=1;i<n;i++)
```

Check remaining meetings.

---

```cpp
if(meetings[i].first > prevEnd)
```

Meeting starts after the previous meeting ends.

---

```cpp
count++;
```

Select it.

---

```cpp
prevEnd = meetings[i].second;
```

Update the latest ending time.

---

```cpp
return count;
```

Return the maximum number of meetings.

---

# 🎯 Why Greedy Works?

Suppose two meetings are available:

```
Meeting A

1 ----- 10
```

```
Meeting B

1 --2
```

Choosing Meeting A blocks almost the entire schedule.

Choosing Meeting B leaves plenty of time for future meetings.

Therefore, selecting the meeting that **ends earliest** always leaves the maximum room for scheduling more meetings later.

This greedy choice can be proven to produce the optimal solution.

---

# 💼 Interview Tips

### Hint 1

If the problem asks to maximize the number of **non-overlapping intervals**, think of sorting by **ending time**.

---

### Hint 2

Do **not** sort by start time.

Ending time determines how quickly the room becomes free.

---