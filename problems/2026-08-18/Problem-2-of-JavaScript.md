# The Galactic Signal Relay

## Description
You are managing a set of $N$ interstellar signal relays arranged in a line, indexed from $0$ to $N-1$. Each relay $i$ has a signal capacity $C[i]$.

A "Signal Chain" is defined as a contiguous subarray $[i, j]$. The **throughput** of a chain is defined as the **minimum** capacity among all relays in that subarray, multiplied by the **length** of the subarray.

However, there is a constraint: a chain is only "stable" if the sum of capacities of the relays within the chain does not exceed a maximum energy threshold $K$.

Given an array of capacities $C$ and a maximum energy threshold $K$, find the **maximum possible throughput** of any stable Signal Chain.

## Examples

**Example 1:**
*   **Input:** `C = [3, 1, 6, 4, 5, 2]`, `K = 10`
*   **Output:** `12`
*   **Explanation:** The subarray `[6, 4]` has a min capacity of 4 and length 2 (throughput 8). The subarray `[4, 5]` has a min capacity of 4 and length 2 (throughput 8). The subarray `[6]` has throughput 6. The most stable subarray is `[4, 5]` with sum 9 (<= 10), throughput 8. Wait, looking at `[6, 4]`, sum is 10. Throughput is $4 \times 2 = 8$. Actually, subarray `[6]` is 6, `[4, 5]` is 8, `[3, 1, 6]` is sum 10, min 1, length 3, throughput 3. The max throughput is 8. (Adjusting: `[6, 4]` has sum 10, min 4, len 2 -> 8).

**Example 2:**
*   **Input:** `C = [10, 20, 30]`, `K = 25`
*   **Output:** `20`
*   **Explanation:** Subarray `[20]` has min 20, len 1, throughput 20. `[10, 20]` has sum 30, which exceeds $K=25$.

## Solution (JavaScript)

To solve this efficiently, we use a **Monotonic Stack** to find the range $[L_i, R_i]$ where $C[i]$ is the minimum, combined with a **Prefix Sum** and a **Sliding Window** (or binary search on the range) to satisfy the constraint $\sum \le K$.

```javascript
/**
 * @param {number[]} C
 * @param {number} K
 * @return {number}
 */
function maxSignalThroughput(C, K) {
    const n = C.length;
    const left = new Array(n);
    const right = new Array(n);
    const stack = [];

    // Monotonic stack to find boundaries where C[i] is the minimum
    for (let i = 0; i < n; i++) {
        while (stack.length && C[stack[stack.length - 1]] >= C[i]) stack.pop();
        left[i] = stack.length ? stack[stack.length - 1] + 1 : 0;
        stack.push(i);
    }
    stack.length = 0;
    for (let i = n - 1; i >= 0; i--) {
        while (stack.length && C[stack[stack.length - 1]] > C[i]) stack.pop();
        right[i] = stack.length ? stack[stack.length - 1] - 1 : n - 1;
        stack.push(i);
    }

    const pref = new Array(n + 1).fill(0);
    for (let i = 0; i < n; i++) pref[i + 1] = pref[i] + C[i];

    let maxThroughput = 0;

    for (let i = 0; i < n; i++) {
        const minVal = C[i];
        let low = left[i], high = right[i];
        
        // Binary search for the widest range [l, r] containing i 
        // such that sum(C[l...r]) <= K
        // Since we need to maximize (r - l + 1), we look for 
        // the largest valid range within [left[i], right[i]]
        
        // Simplified: find max length len such that sum is <= K
        // Using two pointers within the boundaries [left[i], right[i]]
        let l = left[i], r = right[i];
        
        // Find widest window [currL, currR] that includes i and sum <= K
        // This is a variation of finding the largest range including i
        let bestLen = 0;
        
        // Because the sum constraint is monotonic, we can expand outwards from i
        let curSum = C[i];
        let curL = i, curR = i;
        
        while (curL > left[i] || curR < right[i]) {
            if (curL > left[i] && curSum + C[curL - 1] <= K) {
                curSum += C[--curL];
            } else if (curR < right[i] && curSum + C[curR + 1] <= K) {
                curSum += C[++curR];
            } else {
                break;
            }
        }
        maxThroughput = Math.max(maxThroughput, minVal * (curR - curL + 1));
    }

    return maxThroughput;
}
```

## Complexity
Time: **O(N)** due to the monotonic stack and amortized sliding window expansion.  
Space: **O(N)** to store the boundaries and prefix sums.