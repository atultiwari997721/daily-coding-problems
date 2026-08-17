# The Galactic Signal Relay

## Description
You are tasked with managing a series of communication relays in a straight line, indexed from `0` to `n-1`. Each relay `i` has a specific signal strength `s[i]`.

A signal can be transmitted from relay `i` to relay `j` (where `i < j`) if and only if **all** relays between them (i.e., relays `i+1, i+2, ..., j-1`) have a signal strength strictly less than the minimum of `s[i]` and `s[j]`.

Your goal is to find the **total number of valid pairs** `(i, j)` such that a signal can be transmitted directly between them.

## Examples

**Example 1:**
Input: `s = [3, 1, 4]`
Output: `3`
*Explanation: Pairs are (0, 1), (1, 2), (0, 2). All are valid.*

**Example 2:**
Input: `s = [2, 4, 1, 3]`
Output: `4`
*Explanation: Pairs are (0, 1), (0, 2), (1, 3), (2, 3). (0, 3) is invalid because relay 1 (strength 4) is greater than min(2, 3).*

## Solution (Python)

```python
def count_valid_relay_pairs(s):
    """
    A pair (i, j) is valid if all elements between them are smaller 
    than min(s[i], s[j]). This is equivalent to finding visible pairs 
    in a monotonic stack pattern.
    """
    stack = []
    count = 0
    
    for strength in s:
        # While the current relay is stronger than the top of the stack,
        # it can "see" the relay at the top.
        while stack and stack[-1] < strength:
            count += 1
            stack.pop()
        
        # If the stack is not empty, the current relay can "see" 
        # the relay that stopped it from popping (the first one >= strength).
        if stack:
            count += 1
            
        stack.append(strength)
        
    return count
```

## Complexity
Time: **O(n)**, where `n` is the number of relays. Each relay is pushed and popped from the stack at most once.

Space: **O(n)**, to store the monotonic stack in the worst case (e.g., an array sorted in descending order).