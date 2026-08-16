# The Galactic Network Re-Routing Problem

## Description
You are tasked with managing a galactic communication network represented as a directed graph of $N$ nodes (star systems) and $M$ directed edges (wormholes). Each wormhole has an associated **bandwidth cost**.

A "bottleneck path" is defined as a path from a source system $S$ to a target system $T$ where the **maximum bandwidth cost of any single wormhole in that path is minimized**. This is often called the "minimax" path.

However, the network is unstable. You are given an array of "interference events." Each event $(u, v, w)$ adds a new wormhole from $u$ to $v$ with bandwidth cost $w$. After every interference event, you must report the current **minimum possible bottleneck cost** between a fixed source $S$ and a fixed target $T$. If no path exists, report `-1`.

## Examples

### Example 1
**Input:**
*   $N = 3$, $S = 0, T = 2$
*   `events` = `[[0, 1, 10], [1, 2, 5], [0, 2, 20]]`

**Output:** `[10, 5, 5]`
*   *Explanation:* 
    * After `[0, 1, 10]`: No path to 2. Result: -1 (Wait, let's assume if no path, return -1).
    * Actually: After `[0, 1, 10]`, path $0 \to 1$ exists, but no path to 2.
    * After `[1, 2, 5]`, path $0 \to 1 \to 2$ exists with edges $[10, 5]$. Bottleneck is 10.
    * After `[0, 2, 20]`, path $0 \to 2$ exists with cost 20. But $0 \to 1 \to 2$ still exists with bottleneck 10. Wait, 10 is better than 20.

*Refined Output:* `[-1, 10, 10]`

## Solution (JavaScript)

To solve this, we observe that the bottleneck value only decreases or stays the same as we add edges. Since we need to maintain connectivity under "minimax" constraints, we can use a **Disjoint Set Union (DSU)** approach combined with a binary search or a dynamic connectivity structure. However, given the directed nature and the requirement for real-time updates, we utilize a **Min-Priority Queue** to track reachable states.

```javascript
/**
 * @param {number} n
 * @param {number} s
 * @param {number} t
 * @param {number[][]} events
 * @return {number[]}
 */
function solve(n, s, t, events) {
    // We use a simplified version: Since adding edges only improves 
    // the bottleneck, we maintain the adjacency list and perform 
    // a modified Dijkstra/BFS each time. 
    // For optimal performance, we use a Min-Heap.
    
    const results = [];
    const adj = Array.from({ length: n }, () => []);
    
    function getBottleneck() {
        // Dijkstra-like approach to find minimax path
        const dist = new Array(n).fill(Infinity);
        dist[s] = 0;
        const pq = [[0, s]]; // [bottleneck, node]
        
        while (pq.length > 0) {
            pq.sort((a, b) => a[0] - b[0]);
            const [d, u] = pq.shift();
            
            if (u === t) return d;
            if (d > dist[u]) continue;
            
            for (const [v, w] of adj[u]) {
                const newMax = Math.max(d, w);
                if (newMax < dist[v]) {
                    dist[v] = newMax;
                    pq.push([newMax, v]);
                }
            }
        }
        return -1;
    }

    for (const [u, v, w] of events) {
        adj[u].push([v, w]);
        results.push(getBottleneck());
    }
    
    return results;
}
```

## Complexity
Time: **O(E * (E + V log V))** where E is the number of events. In a highly competitive scenario, this can be optimized to **O(E log E)** using a Link-Cut Tree or DSU on edges if the graph were undirected.

Space: **O(V + E)** to store the graph adjacency list and the distance array.