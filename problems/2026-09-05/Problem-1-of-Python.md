# The Galactic Relay Signal

## Description
You are monitoring a space station receiving a stream of signal data represented as a list of integers. Due to interference, the signal sometimes gets "mirrored." A signal is considered **stable** if the sum of all elements at **even indices** is equal to the sum of all elements at **odd indices**.

However, the communication protocol allows you to perform **one optional operation**: you can remove exactly one element from the signal. After removing the element, all elements to the right of it shift one position to the left (potentially changing the indices of the remaining elements).

Given an array `nums`, return the number of indices `i` such that if you remove the element at `nums[i]`, the resulting array becomes **stable**.

## Examples

**Example 1:**
*   **Input:** `nums = [2, 1, 6, 4]`
*   **Output:** `1`
*   **Explanation:** 
    *   Remove index 2 (value 6): Array becomes `[2, 1, 4]`. 
        *   Even indices: `2, 4` (Sum = 6). Odd index: `1` (Sum = 1). Not stable.
    *   Remove index 1 (value 1): Array becomes `[2, 6, 4]`. 
        *   Even indices: `2, 4` (Sum = 6). Odd index: `6` (Sum = 6). **Stable.**
    *   Total stable removals: 1.

**Example 2:**
*   **Input:** `nums = [1, 1, 1]`
*   **Output:** `3`
*   **Explanation:** Removing any element results in `[1, 1]`, which is stable (1 = 1).

## Solution (Python)
```python
def count_stable_removals(nums: list[int]) -> int:
    total_even = sum(nums[i] for i in range(0, len(nums), 2))
    total_odd = sum(nums[i] for i in range(1, len(nums), 2))
    
    count = 0
    curr_even = 0
    curr_odd = 0
    
    for i, val in enumerate(nums):
        # Calculate sums of the remaining array if we remove index i
        # Remaining even sum:
        # Before i: elements at even indices remain even.
        # After i: elements at even indices shift to odd (and vice versa).
        
        after_even = total_even - curr_even - (val if i % 2 == 0 else 0)
        after_odd = total_odd - curr_odd - (val if i % 2 != 0 else 0)
        
        if curr_even + after_odd == curr_odd + after_even:
            count += 1
            
        if i % 2 == 0:
            curr_even += val
        else:
            curr_odd += val
            
    return count
```

## Complexity
Time: **O(n)**, where $n$ is the length of the list, as we iterate through the list twice (once to precompute sums, once to check the condition).
Space: **O(1)**, as we only use a few integer variables to track the running sums.