# The Galactic Relay Signal

## Description
You are tasked with optimizing a communication relay system across a linear array of $N$ space stations, indexed from $0$ to $N-1$. Each station $i$ has a signal strength $S[i]$.

A "relay sequence" is a contiguous subarray of stations where the signal strength is **strictly increasing**. 

However, you have the ability to perform **at most one** "Signal Boost" operation. A Signal Boost allows you to pick any single station $i$ and change its signal strength $S[i]$ to any integer value (you can even set it to match its neighbors).

Given an array of signal strengths $S$, find the length of the **longest possible relay sequence** you can create after performing at most one Signal Boost.

## Examples

**Example 1:**
*   **Input:** `S = [1, 2, 5, 3, 4]`
*   **Output:** `5`
*   **Explanation:** Change $S[2]$ (which is 5) to 2.5 (or any value between 2 and 3). The sequence `[1, 2, 2.5, 3, 4]` is strictly increasing and has length 5.

**Example 2:**
*   **Input:** `S = [10, 20, 5, 6, 7]`
*   **Output:** `4`
*   **Explanation:** You can change $S[2]$ to 21, making the sequence `[10, 20, 21]` (length 3), OR change $S[1]$ to 4, making `[4, 5, 6, 7]` (length 4). The longest is 4.

## Solution (Python)

```python
def longest_relay_sequence(S):
    n = len(S)
    if n <= 2:
        return n
    
    # left[i] stores the length of strictly increasing sequence ending at i
    left = [1] * n
    for i in range(1, n):
        if S[i] > S[i-1]:
            left[i] = left[i-1] + 1
            
    # right[i] stores the length of strictly increasing sequence starting at i
    right = [1] * n
    for i in range(n - 2, -1, -1):
        if S[i] < S[i+1]:
            right[i] = right[i+1] + 1
            
    # Default: best we can do is extend one of the existing sequences by 1
    max_len = 1
    for i in range(n):
        max_len = max(max_len, left[i], right[i])
    
    # Try boosting station i
    # We can bridge sequences if S[i+1] - S[i-1] >= 2
    for i in range(1, n - 1):
        # Case 1: Change S[i] to bridge left[i-1] and right[i+1]
        if S[i+1] - S[i-1] >= 2:
            max_len = max(max_len, left[i-1] + 1 + right[i+1])
        # Case 2: Change S[i] to just extend one side
        max_len = max(max_len, left[i-1] + 1, right[i+1] + 1)
        
    # Boundary cases: boost index 0 or index n-1
    return min(n, max_len + 1)
```

## Complexity
Time: **O(N)**, where N is the number of stations. We traverse the array a constant number of times.
Space: **O(N)** to store the `left` and `right` arrays representing sequence lengths.