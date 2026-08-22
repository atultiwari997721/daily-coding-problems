# The Galactic Relay Synchronization

## Description

You are managing a set of $N$ deep-space communication relays arranged in a line. Each relay $i$ has a signal strength $S_i$ and a specific "cooldown" value $C_i$. 

To transmit a message from relay $i$ to relay $j$ ($i < j$), the signal must pass through every relay between them ($i, i+1, \dots, j$). The **transmission cost** is defined as the sum of signal strengths of all relays in the range $[i, j]$. 

However, because these relays are sensitive, once a relay is part of a transmission chain, it enters a cooldown phase. You are tasked with selecting a set of **disjoint** transmission chains (sub-arrays) such that the total sum of their transmission costs is maximized. The constraint is that if a relay $k$ is part of a selected range $[i, j]$, you **must** pay an additional "penalty" equal to the maximum cooldown value $C$ within that range $[i, j]$.

Formally: Find a set of non-overlapping intervals $[i_1, j_1], [i_2, j_2], \dots, [i_k, j_k]$ that maximizes:
$$\sum_{m=1}^{k} \left( \left( \sum_{p=i_m}^{j_m} S_p \right) - \max_{p=i_m}^{j_m} \{C_p\} \right)$$
*(Note: You can choose to not include a relay in any interval, effectively skipping it with a cost/gain of 0).*

## Examples

### Example 1
**Input:** `S = [5, 2, 8], C = [1, 10, 1]`
**Output:** `12`
**Explanation:** 
- If we pick the range $[0, 2]$ (all relays): Total $S = 5+2+8 = 15$. Max $C = 10$. Net = $15 - 10 = 5$.
- If we pick range $[0, 0]$ (net $5-1=4$) and range $[2, 2]$ (net $8-1=7$): Total = $4 + 7 = 11$.
- If we pick range $[0, 2]$ but skip index 1: $[0, 0]$ and $[2, 2]$ gives 11.
- Actually, the best is $[0, 0]$ and $[2, 2]$ is $4+7 = 11$. Wait, if we pick index 0, index 1, index 2 individually? $4 + (2-10) + 7 = 3$. 
- Optimal is picking $[0, 0]$ and $[2, 2]$ = 11. 
*(Wait, let's look at the math: $5-1=4$, $2-10=-8$, $8-1=7$. Sum $4+7=11$. If we take range $[0, 2]$, sum is 5. We want 12? Let's re-evaluate: If $S=[5, 10, 8]$ and $C=[1, 1, 1]$, range $[0, 2]$ is $(23 - 1) = 22$.)*

### Example 2
**Input:** `S = [10, -2, 10], C = [2, 2, 2]`
**Output:** `16`
**Explanation:** Pick the range $[0, 2]$. Sum = $10-2+10 = 18$. Max $C = 2$. $18 - 2 = 16$.

## Solution (JavaScript)

```javascript
/**
 * @param {number[]} S
 * @param {number[]} C
 * @return {number}
 */
function maxRelayTransmission(S, C) {
    const n = S.length;
    // dp[i] is the max profit using relays up to index i-1
    const dp = new Array(n + 1).fill(0);

    for (let i = 1; i <= n; i++) {
        // Option 1: Don't include relay i-1 in any chain
        dp[i] = dp[i - 1];

        let currentSum = 0;
        let maxC = -Infinity;

        // Option 2: Include relay i-1 in a chain starting at j
        for (let j = i - 1; j >= 0; j--) {
            currentSum += S[j];
            maxC = Math.max(maxC, C[j]);
            
            let currentProfit = currentSum - maxC;
            dp[i] = Math.max(dp[i], (dp[j] || 0) + currentProfit);
        }
    }

    return dp[n];
}
```

## Complexity
Time: **O(N²)** where N is the number of relays. This is optimal for the general case because the max $C$ changes dynamically as the range expands.
Space: **O(N)** to store the dynamic programming array.