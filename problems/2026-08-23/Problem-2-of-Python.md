# The Galactic Relay Network

## Description

You are managing a Galactic Relay Network consisting of $N$ planets, indexed $0$ to $N-1$. Some pairs of planets have bidirectional wormholes connecting them. You are given a list of `edges` where `edges[i] = [u, v, weight]` represents a wormhole between planet $u$ and $v$ with a specific `weight` (latency).

The network is experiencing "Quantum Interference." A path is considered **Stable** if the **bitwise XOR sum** of all weights along the path is exactly $K$.

Given the network, find the **number of unique paths** (by sequence of edges) that start at any planet, end at any planet, and have an XOR sum equal to $K$. 

**Note:**
1. A path may revisit nodes and edges (walks).
2. The path length must be at least 1 edge.
3. Because the network can be infinite in path length, we restrict the path to a maximum of $M$ edges.
4. Return the count modulo $10^9 + 7$.

## Examples

**Example 1:**
*   Input: `N=3, edges=[[0,1,1],[1,2,2]], K=3, M=2`
*   Output: `2`
*   Explanation: 
    *   Path 0 -> 1 (XOR 1) 
    *   Path 1 -> 2 (XOR 2)
    *   Path 0 -> 1 -> 2 (XOR 1 ^ 2 = 3) - **Stable**
    *   Path 2 -> 1 -> 0 (XOR 2 ^ 1 = 3) - **Stable**
    *   Total: 2

**Example 2:**
*   Input: `N=2, edges=[[0,1,5]], K=5, M=3`
*   Output: `3`
*   Explanation:
    *   0 -> 1 (XOR 5) - Stable
    *   1 -> 0 (XOR 5) - Stable
    *   0 -> 1 -> 0 -> 1 (XOR 5^5^5 = 5) - Stable

## Solution (Python)

To solve this efficiently, we use Dynamic Programming with states `dp[length][node][xor_sum]`. Since $M$ can be large, we observe that the XOR values are bounded by the maximum weight in the graph (let $W$ be the max possible XOR value, usually $2^b-1$).

```python
def countStablePaths(N, edges, K, M):
    MOD = 10**9 + 7
    # Find max XOR sum possible
    max_w = 0
    for u, v, w in edges:
        max_w = max(max_w, w)
    
    # XOR sums will not exceed the next power of 2
    limit = 1
    while limit <= max_w or limit <= K:
        limit <<= 1
    
    # dp[xor_sum][node]
    dp = [[0] * N for _ in range(limit)]
    
    # Base case: paths of length 1
    for u, v, w in edges:
        dp[w][u] += 1
        dp[w][v] += 1
        
    total_stable = dp[K][:]
    
    # Iterate for lengths 2 to M
    curr_dp = dp
    for _ in range(2, M + 1):
        next_dp = [[0] * N for _ in range(limit)]
        for u, v, w in edges:
            # Transitions for both directions
            for x in range(limit):
                if curr_dp[x][u] > 0:
                    next_dp[x ^ w][v] = (next_dp[x ^ w][v] + curr_dp[x][u]) % MOD
                if curr_dp[x][v] > 0:
                    next_dp[x ^ w][u] = (next_dp[x ^ w][u] + curr_dp[x][v]) % MOD
        
        curr_dp = next_dp
        for i in range(N):
            total_stable[i] = (total_stable[i] + curr_dp[K][i]) % MOD
            
    return sum(total_stable) % MOD
```

## Complexity
Time: **O(M * E * log(max_W))**, where $E$ is the number of edges and $M$ is the max path length. Given the XOR constraint, this is effectively $O(M \cdot E)$.

Space: **O(N * max_W)** to store the DP table for the current and next path length steps.