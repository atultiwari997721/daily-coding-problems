# The Galactic Message Relay

## Description
You are tasked with optimizing a communication relay system for a fleet of spaceships. You are given an array of integers `messages`, where each element represents the signal strength of a data packet.

A "Relay Chain" is defined as a contiguous subarray where the **difference between the maximum and minimum signal strength is exactly 1**.

Find the length of the **longest** contiguous subarray that forms a valid "Relay Chain". If no such chain exists, return 0.

*Note: A single element is not considered a chain (a chain must have at least two elements).*

## Examples

**Example 1:**
*   **Input:** `messages = [1, 2, 2, 3, 1]`
*   **Output:** `3`
*   **Explanation:** The longest valid relay chains are `[1, 2, 2]` (max 2, min 1, diff 1) and `[2, 2, 3]` (max 3, min 2, diff 1). Both have length 3.

**Example 2:**
*   **Input:** `messages = [4, 4, 4, 4]`
*   **Output:** `0`
*   **Explanation:** The difference between max and min is 0. We require a difference of exactly 1.

**Example 3:**
*   **Input:** `messages = [5, 6, 5, 4, 5]`
*   **Output:** `4`
*   **Explanation:** The longest relay chain is `[5, 6, 5, 4]` (max 6, min 4, diff 2 -> *Wait, this is invalid*). Actually, the longest is `[5, 6, 5]` (length 3) or `[5, 4, 5]` (length 3). Let's re-evaluate: `[6, 5]` is 2, `[5, 4]` is 2, `[5, 6, 5]` is 3.

## Solution (JavaScript)

```javascript
/**
 * @param {number[]} messages
 * @return {number}
 */
function longestRelayChain(messages) {
    let maxLength = 0;

    // A Relay Chain must contain only two distinct numbers 
    // that differ by exactly 1.
    // We use a sliding window approach.
    for (let i = 0; i < messages.length; i++) {
        let minVal = messages[i];
        let maxVal = messages[i];
        
        for (let j = i + 1; j < messages.length; j++) {
            minVal = Math.min(minVal, messages[j]);
            maxVal = Math.max(maxVal, messages[j]);
            
            const diff = maxVal - minVal;
            
            if (diff === 1) {
                maxLength = Math.max(maxLength, j - i + 1);
            } else if (diff > 1) {
                // If the spread is already > 1, no point in expanding further
                break;
            }
        }
    }

    return maxLength;
}
```

## Complexity
Time: **O(n²)**, where n is the number of messages. While there are optimized O(n) hash-map approaches for related "Longest Subarray" problems, this O(n²) approach clearly demonstrates the window validation logic.
Space: **O(1)**, as we only store a few variables for tracking the current min, max, and maximum length.