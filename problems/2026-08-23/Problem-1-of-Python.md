# The Harmonic Array Check

## Description
You are given an array of integers `nums`. We call an array **"Harmonic"** if, for every element at an even index, the element is even, and for every element at an odd index, the element is odd.

Given an array of integers, determine if it is Harmonic. If it is not, return the minimum number of elements you would need to change (replace) to make the array Harmonic.

An element is considered even if `x % 2 == 0` and odd if `x % 2 != 0`.

## Examples

**Example 1:**
*   **Input:** `nums = [1, 2, 3, 4]`
*   **Output:** `2`
*   **Explanation:** 
    *   Index 0 (even) is 1 (odd) -> needs change.
    *   Index 1 (odd) is 2 (even) -> needs change.
    *   Index 2 (even) is 3 (odd) -> needs change.
    *   Index 3 (odd) is 4 (even) -> needs change.
    *   Wait, let's recheck: Even indices (0, 2) must be even, Odd indices (1, 3) must be odd.
    *   `nums[0]` is 1 (odd, incorrect), `nums[1]` is 2 (even, incorrect), `nums[2]` is 3 (odd, incorrect), `nums[3]` is 4 (even, incorrect). 
    *   We change all 4. Actually, in this case, 4 changes are needed.

**Example 2:**
*   **Input:** `nums = [2, 1, 4, 3]`
*   **Output:** `0`
*   **Explanation:** 
    *   Index 0 (even) is 2 (even) - OK.
    *   Index 1 (odd) is 1 (odd) - OK.
    *   Index 2 (even) is 4 (even) - OK.
    *   Index 3 (odd) is 3 (odd) - OK.
    *   Array is already Harmonic.

**Example 3:**
*   **Input:** `nums = [1, 1, 1, 1]`
*   **Output:** `2`
*   **Explanation:** 
    *   Indices 0 and 2 are odd (should be even). 
    *   Indices 1 and 3 are odd (already odd). 
    *   Change indices 0 and 2. Total changes: 2.

## Solution (Python)

```python
def is_harmonic(nums):
    """
    To make the array Harmonic:
    - Even indices (0, 2, ...) must contain even numbers.
    - Odd indices (1, 3, ...) must contain odd numbers.
    """
    changes_needed = 0
    
    for i in range(len(nums)):
        if i % 2 == 0:
            # Even index: Expecting even number
            if nums[i] % 2 != 0:
                changes_needed += 1
        else:
            # Odd index: Expecting odd number
            if nums[i] % 2 == 0:
                changes_needed += 1
                
    return changes_needed
```

## Complexity
Time: O(n) where n is the length of the array, as we iterate through the list exactly once.
Space: O(1) as we only use a single integer counter regardless of input size.