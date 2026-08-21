# The Galactic Signal Filter

## Description
You are monitoring a space telescope that receives a stream of integer signals. Due to cosmic interference, some signals are "echoes" of previous ones. An echo is defined as a signal that is **exactly double** the value of the most recently received signal that has not yet been "cancelled out."

When a signal arrives, perform the following:
1. If the current signal is exactly double the value of the most recent *active* signal, the most recent signal is "cancelled out" (removed from the record), and the current signal is discarded.
2. Otherwise, the current signal is added to the record of active signals.

Return the sum of all active signals remaining in the record after processing the stream.

## Examples

**Example 1:**
- **Input:** `signals = [5, 10, 3, 6, 2]`
- **Output:** `2`
- **Explanation:** 
  - Receive 5. Record: `[5]`
  - Receive 10. 10 is double 5. 5 is cancelled. Record: `[]`
  - Receive 3. Record: `[3]`
  - Receive 6. 6 is double 3. 3 is cancelled. Record: `[]`
  - Receive 2. Record: `[2]`
  - Final sum: 2.

**Example 2:**
- **Input:** `signals = [1, 2, 4, 8]`
- **Output:** `8`
- **Explanation:**
  - 1 and 2 cancel. Record: `[]`
  - 4 and 8 cancel. Record: `[]` (Wait, if 8 is received, 4 is cancelled. If 4 was already gone, 8 stays).
  - Actually: 1 -> `[1]`, 2 -> `[]`, 4 -> `[4]`, 8 -> `[]`. Result 0. 
  - *Wait, if input is [1, 2, 4], 1 & 2 cancel, 4 remains. Result 4.*

## Solution (Python)

```python
def solve(signals):
    stack = []
    
    for s in signals:
        # Check if the stack is not empty and the current signal 
        # is double the top of the stack
        if stack and s == stack[-1] * 2:
            stack.pop()
        else:
            stack.append(s)
            
    return sum(stack)
```

## Complexity
Time: O(n) where n is the length of the signal array, as we iterate through the list exactly once.
Space: O(n) in the worst case where no signals are cancelled and all are stored in the stack.