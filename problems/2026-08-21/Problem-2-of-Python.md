# The Galactic Signal Relay

## Description
You are managing a chain of $N$ space stations arranged in a line, indexed from $0$ to $N-1$. Each station $i$ has a signal strength $S[i]$. 

Due to atmospheric interference, a signal can only be transmitted from station $i$ to station $j$ (where $i < j$) if the signal strength at all stations between them (exclusive of $i$ and $j$) is strictly less than the minimum of $S[i]$ and $S[j]$.

Given an array `S` representing signal strengths, return the **total number of valid pairs** $(i, j)$ such that a signal can be transmitted between them.

## Examples

**Example 1:**
*   **Input:** `S = [3, 1, 2]`
*   **Output:** `3`
*   **Explanation:** Valid pairs are (0,1), (1,2), and (0,2). For (0,2), the intermediate station is index 1 with strength 1. Since $1 < \min(3, 2)$, the condition is satisfied.

**Example 2:**
*   **Input:** `S = [5, 2, 4, 3]`
*   **Output:** `4`
*   **Explanation:** Valid pairs are (0,1), (1,2), (2,3), and (1,3). Note that (0,2) is invalid because the intermediate station (index 1) is 2, which is less than $\min(5, 4)=4$, but (0,3) is invalid because index 1 has strength 2, which is $\le \min(5, 3)=3$. Wait, actually: (0,1), (1,2), (2,3), (1,3) are valid.

## Solution (Python)

This problem can be solved efficiently using a **Monotonic Stack**. For each element, we want to find the nearest element to the left that is greater than or equal to it, and the nearest element to the right that is greater than or equal to it.

```python
def count_valid_relay_pairs(S):
    n = len(S)
    count = 0
    stack = []
    
    # We use a monotonic decreasing stack to find 
    # the next greater element for each index.
    for i in range(n):
        # While current station is stronger than the last one in stack,
        # it can "see" that station.
        while stack and S[stack[-1]] < S[i]:
            count += 1
            stack.pop()
        
        # If there's still something in the stack, the current 
        # station can see the top of the stack as well.
        if stack:
            count += 1
            
        stack.append(i)
        
    return count

# Example usage:
# print(count_valid_relay_pairs([3, 1, 2])) # Output: 3
```

## Complexity
Time: **O(N)**, where N is the number of stations. Each station is pushed onto and popped from the stack at most once.

Space: **O(N)** to store the stack in the worst-case scenario (a strictly decreasing array).