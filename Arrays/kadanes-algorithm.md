# Kadane's Algorithm 
Time Complexity: O(n)

Problem: find the maximum sum of a contiguous subarray within an array of integers. For `maximum_sum([-2, 1, -3, 4, -1, 2, 1, -5, 4])`, the expected result is: `6`, which comes from the subarray `[4, -1, 2, 1]`.

NOTE: For this problem, the smallest possible maximum sum is ZERO,
as we also consider the empty subarray ([], or the 'empty array'), which has a sum of `0` (empty sum).

As an example, suppose we have a negative array, `[-2, -3]`
then, it's possible subarrays are:

```
r = {[], [-2], [-3], [-2, -3]}
max(-2, 0) = 0    
```

### The Algorithm


The main idea is very simple: given the sum of three positive integers `a`, `b`, `c`, we have

```
a + b > a + b - c   
```

(i.) The obvious conclusion, then, is: for all cases of `a + b`, there will never be a positive real number `c`,
such that `(a + b - c)` is greater than `(a + b)`.    

Example 1:

```
[1, -2,  3]
[a, -c,  b] -> notice since we defined c as a positive integer, here it gets its signal flipped!
``` 

For this problem, we can also assume that `[x, y]` is always equivalent to `[y, x]`.

Thus, we can rearrange our example as:

```
[-2, 1, 3]
```

Remember from (i.) that `a + b - c`, cannot be greater than `a + b`   
As such, the sum of `[-2, 1, 3]` will NEVER be greater than the sum of `[1, 3]`

What does this mean, in practice?

(ii.) The maximum possible sum will never contain a negative element, unless it is counterbalanced
by another positive element, greater or equal to its negative equivalent

Example 2 (taken from kata):

```
[-2,  1, -3,  4, -1,  2,  1, -5,  4]
```

From (ii.), its possible to infer `[-2, 1]` will never be part of the maximum sum, 
unless theres another element ahead which counterbalance its current sum!    

CONCLUSION: 

(a) In other words, if the current sum is negative `(cs < 0)`, this element carries NO WEIGHT
UNTIL we reach a element whose value is enough to turn the sum positive again.

(b) If the current sum is greater than the last maximum sum (see NOTE),
then we can update the maximum sum value to the current sum.

```py
def get_maximum_sum(arr):
    # Smallest possible maximum sum is 0
    # and obviously, our current sum starts at 0
    max = 0
    current_sum = 0
    
    for i in arr:
        current_sum += i
        
        # Case (a)
        if current_sum < 0:
            current_sum = 0
            
        # Case (b)
        if current_sum > max:
            max = current_sum
            
    return max
```
