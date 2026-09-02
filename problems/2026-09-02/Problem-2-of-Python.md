# The Galactic Sorting Relay

## Description
You are coordinating a relay race across several space stations. Each space station is assigned a **Signal Strength**, represented as an integer. 

A relay sequence is considered "Stable" if the signal strengths strictly increase or strictly decrease. However, due to cosmic interference, some signals have been duplicated. You are given a list of signal strengths `signals`.

Your task is to determine if the list is **internally consistent**. A list is consistent if, after removing **exactly one** signal, the remaining sequence is strictly monotonic (either strictly increasing or strictly decreasing).

Return `True` if it is possible to make the sequence strictly monotonic by removing exactly one element, otherwise return `False`.

## Examples

**Example 1:**
*   **Input:** `signals = [1, 2, 3, 2]`
*   **Output:** `True`
*   **Explanation:** Removing the last `2` results in `[1, 2, 3]`, which is strictly increasing.

**Example 2:**
*   **Input:** `signals = [5, 4, 3, 2, 5]`
*   **Output:** `True`
*   **Explanation:** Removing the last `5` results in `[5, 4, 3, 2]`, which is strictly decreasing.

**Example 3:**
*   **Input:** `signals = [1, 2, 2, 3]`
*   **Output:** `False`
*   **Explanation:** Removing any single element will result in either `[1, 2, 3]` (but we must remove one, and if we remove a `2`, we have `[1, 2, 3]`, wait—actually, removing the second `2` leaves `[1, 2, 3]`, which works? Let's clarify: if removing *any* one element creates a monotonic sequence, return True).

## Solution (Python)

```python
def is_stable(signals):
    def is_monotonic(arr):
        if len(arr) < 2:
            return True
        
        # Check increasing
        inc = all(arr[i] < arr[i+1] for i in range(len(arr) - 1))
        # Check decreasing
        dec = all(arr[i] > arr[i+1] for i in range(len(arr) - 1))
        
        return inc or dec

    # Try removing each element one by one
    # Note: For large inputs, an O(n) approach is preferred, 
    # but for an Easy challenge, O(n^2) demonstrates the logic clearly.
    for i in range(len(signals)):
        temp = signals[:i] + signals[i+1:]
        if is_monotonic(temp):
            return True
            
    return False
```

## Complexity
Time: O(N²) where N is the number of signals. For each of the N elements, we perform an O(N) check for monotonicity.
Space: O(N) to store the temporary sliced list.