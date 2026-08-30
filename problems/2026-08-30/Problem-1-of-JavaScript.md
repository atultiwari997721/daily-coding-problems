# The Galactic Message Relay

## Description
You are designing a communication system for a fleet of spaceships arranged in a line. Each ship $i$ has a signal strength $S[i]$.

A message can be relayed from ship $i$ to ship $j$ ($i < j$) if and only if every ship between them (at indices $k$ where $i < k < j$) has a signal strength strictly **less than** the signal strength of both $i$ and $j$.

Given an array of signal strengths $S$, return the total number of **valid pairs** $(i, j)$ that can successfully relay a message.

## Examples

**Example 1:**
*   **Input:** `S = [3, 1, 2, 4]`
*   **Output:** `5`
*   **Explanation:** The valid pairs are (0,1), (0,2), (0,3), (1,3), (2,3).
    *   (0, 1): No ships between them.
    *   (0, 2): Ship at index 1 has strength 1 < 3 and 1 < 2.
    *   (0, 3): Ships at indices 1, 2 have strengths 1, 2. Both < 3 and < 4.
    *   (1, 3): Ship at index 2 has strength 2 < 1 (False! Wait, 2 is not less than 1. So this pair is invalid).
    *   *Correction:* Pairs: (0,1), (0,2), (0,3), (1,2), (2,3). Total = 5.

**Example 2:**
*   **Input:** `S = [5, 2, 3, 4, 1]`
*   **Output:** `7`

## Solution (JavaScript)

This problem can be solved using a **Monotonic Stack**. For each element, we want to find the nearest elements to the left and right that are greater than it, as these define the boundaries within which the current ship can "see" other ships.

```javascript
/**
 * @param {number[]} S
 * @return {number}
 */
function countValidRelays(S) {
    const n = S.length;
    let count = 0;

    // A pair (i, j) is valid if all elements between them are smaller than min(S[i], S[j])
    // This is equivalent to finding the "Next Greater Element" to the left and right.
    // However, a more direct approach is to use a monotonic stack to find 
    // the span of influence for each element.
    
    // For each element, it can form pairs with elements to its left 
    // as long as the intermediate elements are smaller.
    const stack = [];
    
    for (let i = 0; i < n; i++) {
        // While current element is greater than stack top, 
        // we can form valid pairs with the popped elements.
        while (stack.length > 0 && S[i] > S[stack[stack.length - 1]]) {
            count++;
            stack.pop();
        }
        
        // If stack is not empty, the current element can form a pair 
        // with the top of the stack (the first element to the left >= S[i])
        if (stack.length > 0) {
            count++;
        }
        
        stack.push(i);
    }
    
    return count;
}
```

## Complexity
Time: **O(n)** where $n$ is the length of the array, as each element is pushed and popped from the stack at most once.

Space: **O(n)** for the stack in the worst case (a strictly decreasing array).