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

**Warning:** please beware that in reality, we need to use `|value| - 1` to find the proper index. We first get the absolute value, as the value could be negative.
But, if we had an element with the same value as the length of the array, we would not be able to index it, as it would excede the length. 
In the example above, it'd be impossible to index an element of value `4` (as there are no elements at index 4), 
despite `4` being a completely valid element according to our rules.

But, how can we use this in our favor?

Since the problem asks us to identify only duplicate items, using the principle described in (i.),
we can "store" numbers which we've previously seen by flipping their signals to ngeative. In other words, if a number is positive, we make it negative. 
The first time we encounter a "flipped" number (negative), it necessarily already have appeared before, and, thus, will be a duplicated. 


```py
def find_first_dup(arr):
    for i in range(len(arr)):
        # Stage 1
        # Retrieve the operation value, which we can obtain by getting the element indexed at i, and applying our formula
        index_from_value = abs(arr[i]) - 1 

        # Stage 2
        if arr[index_from_value] < 0:
            return i
        
        # Stage 3
        arr[index_from_value] = - arr[index_from_value]
```

So, for example, let's go through the array from Example 2 (shortened version here):


| Value | 2 | 3 | 4 | 1 | 3 | 
| --- | --- | --- | --- | --- | --- |
| Index | 0 | 1 | 2 | 3 | 4 |

We start at `i = 0`, from where we obtain an "operation index" (`index_from_value = 1`), by applying the formula `|val| - 1`.
   * At `arr[index_from_value]`, we check to see if the value is negative. 
    Since `arr[1] = 3`, we turn it into `-3`.

| Value | 2 | [-3] | 4 | 1 | 3 | 
| --- | --- | --- | --- | --- | --- |
| Index | >0< | 1 | 2 | 3 | 4 |

We continue to `i = 1`, where we get (`index_from_value = 2`), by applying the formula `|3| - 1`.
    Since `arr[2] = 4`, we turn it into `-4`.

| Value | 2 | -3 | [-4] | 1 | 3 | 
| --- | --- | --- | --- | --- | --- |
| Index | 0 | >1< | 2 | 3 | 4 |

In `i = 2`, we get (`index_from_value = 3`) from `|4| - 1`.
    Since `arr[3] = 1`, we turn it into `-1`.

| Value | 2 | -3 | -4 | [-1] | 3 | 
| --- | --- | --- | --- | --- | --- |
| Index | 0 | 1 | >2< | 3 | 4 |

In `i = 3`, we get (`index_from_value = 0`) from `|1| - 1`.
    Since `arr[0] = 2`, we turn it into `-2`.


| Value | [-2] | -3 | -4 | -1 | 3 | 
| --- | --- | --- | --- | --- | --- |
| Index | 0 | 1 | 2 | >3< | 4 |

In `i = 4`, we get (`index_from_value = 2`) from `|3| - 1`
    But wait, `arr[2] = -4`. This is a negative number, which means we already saw it before!
    Since `arr[2]` is negative, we return the current index `i = 4`


| Value | -2 | -3 | [-4] | -1 | 3 | 
| --- | --- | --- | --- | --- | --- |
| Index | 0 | 1 | 2 | 3 | >4< |
