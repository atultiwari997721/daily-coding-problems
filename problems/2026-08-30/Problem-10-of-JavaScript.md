# The Harmonic Server Load Balancer

## Description

You are building a load balancer for a high-traffic distributed system. You are given an array of integers `servers`, where `servers[i]` represents the current CPU load of the $i$-th server. 

A "Harmonic Cluster" is defined as a **contiguous subarray** where the absolute difference between any two elements in that subarray is **less than or equal to 1**. 

Find the length of the **longest** Harmonic Cluster in the server array.

## Examples

**Example 1:**
*   **Input:** `servers = [4, 5, 4, 3, 3, 4]`
*   **Output:** `4`
*   **Explanation:** The longest Harmonic Cluster is `[4, 3, 3, 4]` (or `[5, 4, 3, 3]`), which has a length of 4. Any two elements have a difference of $\le 1$.

**Example 2:**
*   **Input:** `servers = [1, 2, 8, 9, 10]`
*   **Output:** `2`
*   **Explanation:** Possible clusters include `[1, 2]` or `[8, 9]` or `[9, 10]`, all length 2.

**Example 3:**
*   **Input:** `servers = [7, 7, 7]`
*   **Output:** `3`
*   **Explanation:** All elements are the same, so the difference is 0, which is $\le 1$.

## Solution (JavaScript)

To solve this efficiently, we use a sliding window approach with a `Map` to track the frequencies of the numbers currently in our window. Since a Harmonic Cluster can only contain at most two distinct values (e.g., $x$ and $x+1$, or $x$ and $x$), we ensure the window remains valid as we expand.

```javascript
/**
 * @param {number[]} servers
 * @return {number}
 */
function longestHarmonicCluster(servers) {
    let left = 0;
    let maxLength = 0;
    const freqMap = new Map();

    for (let right = 0; right < servers.length; right++) {
        // Add current server to the frequency map
        freqMap.set(servers[right], (freqMap.get(servers[right]) || 0) + 1);

        // While the difference between max and min keys in map > 1, shrink window
        while (Math.max(...freqMap.keys()) - Math.min(...freqMap.keys()) > 1) {
            const leftVal = servers[left];
            freqMap.set(leftVal, freqMap.get(leftVal) - 1);
            
            if (freqMap.get(leftVal) === 0) {
                freqMap.delete(leftVal);
            }
            left++;
        }

        // Calculate max length
        maxLength = Math.max(maxLength, right - left + 1);
    }

    return maxLength;
}
```

## Complexity
**Time:** $O(N)$, where $N$ is the length of the `servers` array. While the `Math.max(...freqMap.keys())` call inside the loop looks like it could be $O(K)$ where $K$ is the number of keys, $K$ is strictly bounded by 3 (if $K > 2$, the difference condition is violated), making the operations inside the loop effectively $O(1)$.

**Space:** $O(1)$, because the `freqMap` will contain at most 3 entries at any given time due to the sliding window logic.