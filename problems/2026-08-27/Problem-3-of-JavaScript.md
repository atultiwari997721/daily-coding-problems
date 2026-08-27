# The Galactic Relay Signal

## Description
You are managing a chain of $n$ deep-space communication relays arranged in a line, indexed from $0$ to $n-1$. Each relay $i$ has a signal strength $S[i]$.

A "stable transmission" occurs between relay $i$ and relay $j$ (where $i < j$) if the average signal strength of all relays in the range $[i, j]$ is exactly equal to the average of the signal strengths of the endpoints, i.e., $(S[i] + S[j]) / 2$. 

However, since we are dealing with integer signal strengths, we define a "balanced relay segment" as a segment $[i, j]$ where $j > i$ and the **sum** of all elements in the segment $[i, j]$ is equal to the sum of the endpoints multiplied by the number of elements in the segment divided by two:
$$\sum_{k=i}^{j} S[k] = \frac{(S[i] + S[j]) \times (j - i + 1)}{2}$$

Essentially, this means the elements in the segment must form an **Arithmetic Progression**.

Given an array of integers `signalStrengths`, return the number of balanced relay segments of length at least 2.

## Examples

**Example 1:**
*   **Input:** `signalStrengths = [1, 2, 3, 4, 5]`
*   **Output:** `10`
*   **Explanation:** Every contiguous subarray of length $\ge 2$ is an arithmetic progression (e.g., [1,2], [2,3,4], [1,2,3,4,5]). There are $5(6)/2 - 5 = 10$ such segments.

**Example 2:**
*   **Input:** `signalStrengths = [1, 3, 5, 2, 4, 6]`
*   **Output:** `6`
*   **Explanation:** The balanced segments are [1,3,5], [3,5], [1,3], [2,4,6], [4,6], and [2,4].

## Solution (JavaScript)

```javascript
/**
 * @param {number[]} signalStrengths
 * @return {number}
 */
function countBalancedRelays(signalStrengths) {
    const n = signalStrengths.length;
    if (n < 2) return 0;

    let totalCount = 0;
    let currentLen = 0;

    // A segment of length 2 is always an arithmetic progression.
    // For length > 2, a segment is an AP if the difference between 
    // adjacent elements remains constant.
    for (let i = 2; i < n; i++) {
        if (signalStrengths[i] - signalStrengths[i - 1] === signalStrengths[i - 1] - signalStrengths[i - 2]) {
            currentLen++;
        } else {
            currentLen = 0;
        }
        totalCount += currentLen;
    }

    // Add all segments of length 2
    return totalCount + (n - 1);
}
```

## Complexity
Time: **O(n)** where $n$ is the length of the array, as we traverse the array once.
Space: **O(1)** as we only use a few integer variables to track the current count and length.