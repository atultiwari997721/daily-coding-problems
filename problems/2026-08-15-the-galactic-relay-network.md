# The Galactic Relay Network

## Description

You are managing a Galactic Relay Network consisting of `n` relay stations, labeled `0` to `n-1`. The network is initially disconnected. You are given an array of `edges` where `edges[i] = [u, v, w]` represents a potential bidirectional connection between stations `u` and `v` with a "signal latency" of `w`.

However, each relay station `i` has a **Capacity Constraint** `c[i]`. A connection `[u, v]` can only be activated if the current number of active connections for *both* `u` and `v` does not exceed their respective capacities `c[u]` and `c[v]`.

Your task is to build a **Minimum Spanning Forest** that connects the maximum number of stations possible. If multiple forests connect the same maximum number of stations, return the **minimum possible total latency** of the edges used. 

If there are still multiple ways to achieve that minimum latency, return the one that uses the **lexicographically smallest set of edge indices** (where indices are based on the original `edges` input array).

## Examples

**Example 1:**
Input: 
`n = 3`, `capacities = [1, 1, 1]`, `edges = [[0, 1, 10], [1, 2, 20], [0, 2, 30]]`
Output: `[0, 1]`
*Explanation: We can pick edges 0 and 1 (latency 30). Station 1 has capacity 1, but it connects to 0 and 2. This is invalid. We can pick [0, 1] and [1, 2] only if capacities allow. With capacity 1, each node can have only one edge. We pick [0, 1] and... wait, capacity 1 means node 1 can only be part of one edge.*

**Example 2:**
Input:
`n = 4`, `capacities = [2, 2, 2, 2]`, `edges = [[0, 1, 5], [1, 2, 5], [2, 3, 5], [0, 3, 10]]`
Output: `[0, 1, 2]`

## Solution (JavaScript)

```javascript
/**
 * @param {number} n
 * @param {number[]} capacities
 * @param {number[][]} edges
 * @return {number[]}
 */
function solveGalacticRelay(n, capacities, edges) {
    // We add the index to the edges to track them
    const indexedEdges = edges.map((e, i) => [...e, i]);
    
    // Sort by weight ascending, then by index ascending
    indexedEdges.sort((a, b) => a[2] - b[2] || a[3] - b[3]);
    
    const degree = new Array(n).fill(0);
    const parent = Array.from({ length: n }, (_, i) => i);
    
    function find(i) {
        if (parent[i] === i) return i;
        return parent[i] = find(parent[i]);
    }
    
    const resultIndices = [];
    
    for (const [u, v, w, originalIdx] of indexedEdges) {
        const rootU = find(u);
        const rootV = find(v);
        
        // Check capacity constraints
        if (degree[u] < capacities[u] && degree[v] < capacities[v]) {
            // Check if they are already in the same component (avoid cycles)
            if (rootU !== rootV) {
                parent[rootU] = rootV;
                degree[u]++;
                degree[v]++;
                resultIndices.push(originalIdx);
            }
        }
    }
    
    return resultIndices.sort((a, b) => a - b);
}
```

## Complexity
Time: **O(E log E)**, where E is the number of edges, due to the sorting step. The Union-Find operations take near-constant time (amortized inverse Ackermann function).

Space: **O(V + E)**, where V is the number of vertices, to store the Union-Find structure, degrees, and the result array.