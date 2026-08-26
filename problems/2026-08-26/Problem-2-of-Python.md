# The Echo Chamber Shift

## Description

You are given a list of integers `nums` representing a sequence of audio samples. Due to a recording glitch, the signal has been "shifted" circularly. An **Echo Chamber Shift** of size `k` means the sequence has been rotated `k` positions to the right. 

However, before the shift occurred, someone applied a "Mirror Effect": every element `x` was replaced by `target - x` (where `target` is a fixed constant).

Given the shifted, mirrored array `nums`, a target value `target`, and the shift amount `k`, **reconstruct the original array** before the shift and mirroring occurred.

*Note: The original array contained non-negative integers.*

## Examples

**Example 1:**
*   **Input:** `nums = [5, 2, 8]`, `target = 10`, `k = 1`
*   **Process:**
    1.  Reverse the shift (right shift by 1): `[8, 5, 2]`
    2.  Reverse the mirror (`10 - x`): `[10-8, 10-5, 10-2]` = `[2, 5, 8]`
*   **Output:** `[2, 5, 8]`

**Example 2:**
*   **Input:** `nums = [0, 4]`, `target = 4`, `k = 0`
*   **Output:** `[4, 0]`

## Solution (Python)

```python
def reconstruct_original(nums, target, k):
    n = len(nums)
    if n == 0:
        return []
    
    # Normalize k to handle shifts larger than array length
    k = k % n
    
    # 1. Reverse the circular shift:
    # A right shift by k means the element at index i moved to (i + k) % n.
    # To undo a right shift, we perform a left shift by k.
    # The element at index i in the original array is now at index (i + k) % n.
    # So, original[i] = nums[(i + k) % n]
    
    unshifted = [0] * n
    for i in range(n):
        unshifted[i] = nums[(i + k) % n]
        
    # 2. Reverse the mirror effect:
    # Original[i] = target - nums_after_mirror[i]
    return [target - x for x in unshifted]
```

## Complexity
Time: O(n), where n is the length of the list, as we iterate through the list twice.
Space: O(n), to store the reconstructed list.