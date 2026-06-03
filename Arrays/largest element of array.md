# Largest Element in an Array

Given an array of integers, find the largest element in the array.

Approach : 
Assume the first element is the largest.
Traverse the array from left to right.
If the current element is greater than the current maximum, update the maximum.
Return the maximum after traversing the entire array.

# Mistakes I Made While Solving

## 1. Forgot the type of parameter n

Incorrect:

void find_largest(int arr[], n)

Correct:

int find_largest(int arr[], int n)

## 2. Used the wrong comparison operator

Incorrect:

if(arr[i] < max)

Correct:

if(arr[i] > maxElement)

## 3. Returned inside the loop

Incorrect:

for(...)
{
    return max;
}

This causes the function to exit after the first iteration.

Correct:

for(...)
{
    ...
}
return maxElement;

## 4. Used arr.size() on a C-style array

Incorrect:

int n = arr.size();

Correct:

int n = sizeof(arr) / sizeof(arr[0]);

## Code : 
```cpp 
#include <bits/stdc++.h>
using namespace std;

int find_largest(int arr[], int n){

    int max = arr[0];

    for(int i = 0; i < n; i++){
        if(arr[i] > max){
            max = arr[i];
        }
    }
    return max;
}

int main(){
    int arr[] = {34, 65, 0, 23, 44, 61, 11};
    int n = sizeof(arr)/sizeof(arr[0]);

    int max = find_largest(arr, n);
    cout << "Largest Element of Array is: " << max;

    return 0;
}
```
## Time Complexity

Time Complexity: O(n)

Space Complexity: O(1)

Since the array is traversed only once, this is the optimal solution.
