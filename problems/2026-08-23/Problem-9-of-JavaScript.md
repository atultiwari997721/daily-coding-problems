# The Galactic Signal Relay

## Description
You are designing a communication system for a fleet of starships positioned in a line. Each ship $i$ has a signal strength $S[i]$.

A group of ships forms a "Coherent Relay" if, for every ship in the group, its signal strength is strictly greater than the signal strength of its immediate neighbors **within that specific group**. 

Wait—that's not quite right for signal propagation! Let's redefine: A group of ships $[i, i+1, \dots, j]$ is a **"Resonant Chain"** if the signal strengths are **strictly alternating** in parity (even, odd, even, odd OR odd, even, odd, even).

Given an array of integers `S` representing the signal strengths of ships, find the **length of the longest Resonant Chain**.

## Examples

**Example 1:**
*   **Input:** `S = [1, 2, 3, 4, 5]`
*   **Output:** `5`
*   **Explanation:** The entire array `[1, 2, 3, 4, 5]` is a Resonant Chain because the parity alternates (Odd, Even, Odd, Even, Odd).

**Example 2:**
*   **Input:** `S = [10, 12, 14, 7, 8]`
*   **Output:** `3`
*   **Explanation:** The longest chains are `[14, 7, 8]`. The parity sequence is (Even, Odd, Even), which alternates.

**Example 3:**
*   **Input:** `S = [2, 4, 6, 8]`
*   **Output:** `1`
*   **Explanation:** No two adjacent ships have different parity. The longest chain is any single ship.

## Solution (JavaScript)

```javascript
/**
 * @param {number[]} S
 * @return {number}
 */
function longestResonantChain(S) {
    if (S.length === 0) return 0;
    
    let maxLength = 1;
    let currentLength = 1;
    
    for (let i = 1; i < S.length; i++) {
        // Check if the current ship's parity is different from the previous one
        // Parity is the same if (a % 2 === b % 2)
        // We use Math.abs to handle potential negative signal values
        if (Math.abs(S[i] % 2) !== Math.abs(S[i - 1] % 2)) {
            currentLength++;
        } else {
            currentLength = 1;
        }
        
        maxLength = Math.max(maxLength, currentLength);
    }
    
    return maxLength;
};
```

## Complexity
Time: **O(N)**, where N is the number of ships, as we iterate through the array exactly once.
Space: **O(1)**, as we only use a few variables to track the current and maximum chain lengths.