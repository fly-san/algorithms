# Find the first duplicate element
Time Complexity: O(n)

Problem: create a function which returns the index of the first duplicated element within an array. 

NOTE: The elements in this array are positive integers, such that their maximum possible value is the same as the array's length. 
Therefore, there cannot exist an array such as `[1, 3, 5, 2]`, the element `5` is larger than the length `4`.

Example 1:

 `find_duplicate([1, 3, 2, 2, 4, 3])` is expected to return `3` (assuming index starts at 0), as the earliest occurrence of a duplicated number is at position `3`.

### The Algorithm

The key to answer this question is to realize the importance of the restrictions proposed by the NOTE. 
Notice that **if no element can be larger than the length of the array, it is always possible to index this array using any given element.**

Example 2:
Suppose we have an array (of length `5`), `arr = [2, 3, 4, 1, 3]`

| Value | 2 | 3 | 4 | 1 | 3 | 
| --- | --- | --- | --- | --- | --- |
| Index | 0 | 1 | 2 | 3 | 4 | 

If we, for example, take the element with a value of `2` (indexed at 0) and use it as the index, we get the element `3` (indexed at 2). 

If, in turn, we get the first element with a value of `3` (indexed at 1) and index it, we get the element `1` (indexed at 3). 
However, notice that this also happens for the second element with a value of `3` (indexed at 4).

(i.) It's trivial, then, to realize that for every element with the same value, if we index the array at said value, we will always get the same element.

**Warning:** please beware that in reality, we need to use `value - 1` to find the proper index. 
Since if we have an element with the same value as the length of the array, we will not be able to index it. 
In the example above, it'd be impossible to index an element of value `4` (as there are no elements at index 4), 
despite `4` being a completely valid element according to our rules.

But, how can we use this in our favor?

Since the problem asks us to identify only duplicate items, using the principle described in (i.),
we can "store" numbers which we've previously seen by flipping their signals to ngeative. In other words, if a number is positive, we make it negative. 
The first time we encounter a "flipped" number (negative), it necessarily already have appeared before, and, thus, will be a duplicated. 

| Value | 3 | 4 | 3 | 
| --- | --- | --- | --- |
| Index | 1 | 2 | 4 | 

So, for example, let's go through the array from Example 2 (shortened version here):

1) At position `1`, we find an element with value `3`. 
  We apply our formula (see warning), `result_index = value - 1`, and get `result_index = 2`.

  - At position `2`, we find the value `4`. We then flip the value `4` to `-4`. The array now looks like this


| Value | 3 | -4 | 3 | 
| --- | --- | --- | --- |
| Index | 1 | 2 | 4 | 

  - We continue to the next position.

2) At position `2`, we find an element with value `-4`.

  - Since this value is negative, we return it, as it is the first duplicated value we encountered. 
  - Return the current position.


```py
def find_first_duplicate(arr):
    for i in range(len(arr)):

        # Case 2
        if arr[i] < 0:
            return i

        # Case 1
        absolute_i = abs(arr[i]) 
        arr[i] = - arr[i]
```
