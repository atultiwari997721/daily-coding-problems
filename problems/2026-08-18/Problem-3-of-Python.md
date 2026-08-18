# The Galactic Signal Echo

## Description
You are analyzing signals from deep space. You receive a sequence of integers representing the "intensity" of the signal at different intervals. 

A signal is considered **"Echo-Balanced"** if the intensity at the beginning of the sequence is equal to the intensity at the end of the sequence. Given an array of integers `signals`, your task is to find the **maximum length** of a contiguous subarray that is "Echo-Balanced." 

If no such subarray exists (where the start and end values are the same), return 0. Note that a single element is technically a subarray where the start equals the end, so its length is 1.

## Examples

**Example 1:**
*   **Input:** `signals = [1, 5, 2, 1, 3, 5]`
*   **Output:** `5`
*   **Explanation:** The subarray `[1, 5, 2, 1]` is Echo-Balanced (starts and ends with 1), length 4. The subarray `[5, 2, 1, 3, 5]` is also Echo-Balanced (starts and ends with 5), length 5. The max length is 5.

**Example 2:**
*   **Input:** `signals = [4, 4, 4, 4]`
*   **Output:** `4`
*   **Explanation:** The whole array is Echo-Balanced.

**Example 3:**
*   **Input:** `signals = [1, 2, 3, 4]`
*   **Output:** `1`
*   **Explanation:** No two elements are the same, but individual elements are Echo-Balanced (length 1).

## Solution (Python)
```python
def max_echo_balanced_length(signals):
    if not signals:
        return 0
    
    # Store the first occurrence index of each signal value
    first_occurrence = {}
    max_len = 0
    
    for i, val in enumerate(signals):
        if val not in first_occurrence:
            first_occurrence[val] = i
        
        # Calculate current length: (current_index - first_index + 1)
        current_len = i - first_occurrence[val] + 1
        max_len = max(max_len, current_len)
            
    return max_len
```

## Complexity
Time: **O(N)**, where N is the length of the input array. We traverse the array exactly once.
Space: **O(K)**, where K is the number of unique elements in the array stored in our hash map. In the worst case, K = N.