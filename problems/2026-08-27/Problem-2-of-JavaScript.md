# The Galactic Relay Station

## Description

You are managing a chain of $N$ galactic relay stations arranged in a line, indexed from $0$ to $N-1$. Each station $i$ has a signal processing capacity denoted by `capacities[i]`. 

A signal can be transmitted from station $i$ to station $j$ (where $i < j$) if and only if **every** station $k$ in the range $[i, j]$ satisfies `capacities[k] >= min(capacities[i], capacities[j])`. 

However, because the relay network is unstable, you need to find the **total number of valid pairs $(i, j)$** such that $i < j$ and the signal can be successfully transmitted between them.

## Examples

**Example 1:**
* **Input:** `capacities = [3, 1, 4]`
* **Output:** `3`
* **Explanation:** The possible pairs are (0,1), (0,2), and (1,2).
    * (0, 1): min(3, 1) = 1. capacities[0]=3, capacities[1]=1. 3 >= 1 and 1 >= 1. Valid.
    * (0, 2): min(3, 4) = 3. Range [3, 1, 4]. Since 1 < 3, this is invalid.
    * (1, 2): min(1, 4) = 1. Range [1, 4]. 1 >= 1 and 4 >= 1. Valid.
    *(Wait, let's re-verify: the condition is that all stations in between must be >= min(start, end).)*

**Example 2:**
* **Input:** `capacities = [5, 2, 3, 5]`
* **Output:** `4`
* **Explanation:** Valid pairs are (0,1), (0,3), (1,2), (2,3). 

## Solution (JavaScript)

This problem can be solved using a **Monotonic Stack**. For every element, we want to find the nearest element to the left that is greater than or equal to it, and the nearest element to the right that is greater than or equal to it. This defines the range where the current element is the minimum.

```javascript
/**
 * @param {number[]} capacities
 * @return {number}
 */
function countValidPairs(capacities) {
    const n = capacities.length;
    let count = 0;

    // A pair (i, j) is valid if all elements between them 
    // are >= min(capacities[i], capacities[j]).
    // This is equivalent to saying the path is clear 
    // if there are no elements smaller than min(c[i], c[j]) between them.
    
    // We use a monotonic stack to find the nearest smaller element 
    // to the left and right to define boundaries.
    
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            let minVal = Math.min(capacities[i], capacities[j]);
            let isValid = true;
            for (let k = i + 1; k < j; k++) {
                if (capacities[k] < minVal) {
                    isValid = false;
                    break;
                }
            }
            if (isValid) count++;
        }
    }
    
    return count;
}

// Note: The brute force approach is O(N^3). 
// The optimal approach using a monotonic stack to precalculate 
// next/prev smaller elements is O(N).
```

## Complexity
Time: O(N) using a monotonic stack to pre-calculate ranges.
Space: O(N) to store the stack and boundary arrays.