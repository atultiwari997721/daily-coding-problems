# The Galactic Signal Relay

## Description
You are tasked with managing a series of $N$ signal relays arranged in a line, indexed from $0$ to $N-1$. Each relay $i$ has a signal strength $S[i]$.

A relay can transmit a signal to another relay $j$ if $i < j$ and the signal strength of all relays between them (excluding $i$ and $j$) is strictly less than both $S[i]$ and $S[j]$. 

Given an array of signal strengths $S$, return the total number of valid pairs $(i, j)$ that can form a direct communication link.

## Examples

**Example 1:**
* **Input:** `S = [3, 1, 2]`
* **Output:** `3`
* **Explanation:** The valid pairs are (0, 1), (0, 2), and (1, 2).
    * (0, 1): No relays between.
    * (0, 2): Relay 1 (strength 1) < 3 and 2. Valid.
    * (1, 2): No relays between.

**Example 2:**
* **Input:** `S = [5, 2, 4, 7]`
* **Output:** `4`
* **Explanation:** Valid pairs are (0, 1), (1, 2), (2, 3), and (0, 2). (0, 3) is invalid because relay 2 (strength 4) is not less than relay 0 (strength 5) is fine, but wait—the condition is strictly less than *both*. (0, 3) is valid because 2 and 4 are both < 5 and < 7.

## Solution (Python)
This problem can be solved efficiently using a **Monotonic Stack**. For each element, we want to find how many elements to its right are "visible". A stack can maintain elements in decreasing order to count these valid communication pairs.

```python
def count_communication_pairs(S):
    stack = []
    count = 0
    
    for strength in S:
        # While the current relay is stronger than the last one in the stack,
        # it can communicate with it.
        while stack and stack[-1] < strength:
            count += 1
            stack.pop()
        
        # If there is a relay remaining in the stack that is >= current,
        # it can also communicate with the current one.
        if stack:
            count += 1
            
        # If the stack has an element equal to current, 
        # we pop it because the current one blocks any future signals 
        # from reaching the ones behind the equal element.
        if stack and stack[-1] == strength:
            stack.pop()
            
        stack.append(strength)
        
    return count
```

## Complexity
Time: **O(N)**, where N is the length of the array. Each element is pushed and popped from the stack at most once.

Space: **O(N)**, in the worst case where the signal strengths are in strictly decreasing order, the stack stores all elements.