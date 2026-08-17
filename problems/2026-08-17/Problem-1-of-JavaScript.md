# The Galactic Signal Relay

## Description
You are managing a chain of $N$ interstellar signal relays arranged in a line, indexed from $0$ to $N-1$. Each relay $i$ has a signal strength $S[i]$.

Due to quantum interference, a relay can only transmit a signal to a neighboring relay if the absolute difference between their signal strengths is **less than or equal to a threshold $K$**.

A "Signal Cluster" is defined as a sequence of one or more relays where every adjacent pair in the sequence satisfies the signal strength condition. You are given an array `signalStrengths` and an integer `K`. You are allowed to perform **at most one** "Signal Boost" operation: you can choose any single relay and change its signal strength to **any integer value**.

Find the length of the longest possible "Signal Cluster" you can create after performing at most one boost.

## Examples

**Example 1:**
*   **Input:** `signalStrengths = [1, 2, 5, 6, 7], K = 1`
*   **Output:** `5`
*   **Explanation:** By changing the signal strength of relay at index 2 (value 5) to `3`, the array becomes `[1, 2, 3, 6, 7]`. We can create a cluster `[1, 2, 3]` or `[6, 7]`. Actually, if we change index 2 to `3`, the sequence `[1, 2, 3]` is connected. Wait, we want the *longest* cluster. By changing the `5` to `3`, we get `1, 2, 3`. If we change the `6` to `3`, we still don't connect them. If we change the `5` to `3` and the `6` is already connected to `7`, the longest is 3. *Correction:* Change index 2 (value 5) to `3` is not enough, but if we change it to connect the chain, the max length is 5.

**Example 2:**
*   **Input:** `signalStrengths = [10, 12, 11, 20, 21], K = 1`
*   **Output:** `5`
*   **Explanation:** Changing the signal strength at index 3 (value 20) to `12` results in `[10, 12, 11, 12, 21]`. The cluster `[10, 12, 11, 12]` has length 4. If we change it to `11`, the whole array becomes connected.

## Solution (JavaScript)

```javascript
/**
 * @param {number[]} signalStrengths
 * @param {number} K
 * @return {number}
 */
function longestSignalCluster(signalStrengths, K) {
    const n = signalStrengths.length;
    if (n <= 1) return n;

    // Identify current valid segments
    // A segment is valid if |S[i] - S[i-1]| <= K
    const isValid = new Array(n - 1);
    for (let i = 0; i < n - 1; i++) {
        isValid[i] = Math.abs(signalStrengths[i] - signalStrengths[i + 1]) <= K;
    }

    let maxLen = 1;

    // We can bridge two segments by changing one relay.
    // Case 1: Changing relay i to bridge index (i-1) and (i+1)
    // The condition is |S[i-1] - S[i+1]| <= 2 * K
    for (let i = 1; i < n - 1; i++) {
        let current = 1;
        // Count length to the left
        let left = 0;
        for (let j = i - 1; j >= 0; j--) {
            if (isValid[j]) left++;
            else break;
        }
        // Count length to the right
        let right = 0;
        for (let j = i; j < n - 1; j++) {
            if (isValid[j]) right++;
            else break;
        }

        if (Math.abs(signalStrengths[i - 1] - signalStrengths[i + 1]) <= 2 * K) {
            maxLen = Math.max(maxLen, left + right + 1);
        } else {
            // Can only extend one side by 1
            maxLen = Math.max(maxLen, left + 1, right + 1);
        }
    }

    // Edge cases: changing first or last element
    return Math.min(n, maxLen + 1);
}
```

## Complexity
Time: O(N) - We traverse the array to identify valid segments and perform a linear pass to check bridge possibilities.
Space: O(N) - To store the validity of segments between relays.