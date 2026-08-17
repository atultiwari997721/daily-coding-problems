# The Galactic Relay Network

## Description

You are managing a Galactic Relay Network consisting of `n` relay stations, labeled `0` to `n-1`. Some stations are connected by bidirectional quantum links, represented by an array `edges` where `edges[i] = [u, v]`.

However, the network is unstable. A "critical path" is defined as any simple path between two stations. A relay station is considered **vital** if, by removing it (and all links connected to it), the number of connected components in the remaining network increases. 

Your task is to find the **weighted sum of vital stations**. Each station `i` has a base importance `weights[i]`. The "vulnerability score" of a vital station is defined as the maximum number of connected components created by its removal. 

Return the sum of `weights[i] * vulnerability_score(i)` for all vital stations in the network. If a station is not vital, its vulnerability score is 0.

## Examples

**Example 1:**
*   **Input:** `n = 5`, `edges = [[0,1],[1,2],[2,0],[1,3],[3,4]]`, `weights = [10, 20, 10, 30, 10]`
*   **Output:** `80`
*   **Explanation:** 
    *   Removing station `1` splits the graph into `{0,2}`, `{3}`, `{4}` (3 components). Score: `20 * 3 = 60`.
    *   Removing station `3` splits the graph into `{0,1,2}`, `{4}` (2 components). Score: `30 * 2 = 60`? No, wait: Removing `3` leaves `{0,1,2}` and `{4}`. Score: `30 * 2 = 60`.
    *   Total: `60 + 60 = 120`. (Actually, let's re-evaluate: removing 1 leaves 3 components, removing 3 leaves 2 components. 20*3 + 30*2 = 120).

## Solution (JavaScript)

```javascript
/**
 * @param {number} n
 * @param {number[][]} edges
 * @param {number[]} weights
 * @return {number}
 */
function solve(n, edges, weights) {
    const adj = Array.from({ length: n }, () => []);
    for (const [u, v] of edges) {
        adj[u].push(v);
        adj[v].push(u);
    }

    const tin = new Array(n).fill(-1);
    const low = new Array(n).fill(-1);
    const children = new Array(n).fill(0);
    const vulnerability = new Array(n).fill(0);
    let timer = 0;

    // We use Tarjan's/Hopcroft-Tarjan algorithm concept for articulation points
    function dfs(u, p = -1) {
        tin[u] = low[u] = timer++;
        let componentsCreated = 0;

        for (const v of adj[u]) {
            if (v === p) continue;
            if (tin[v] !== -1) {
                low[u] = Math.min(low[u], tin[v]);
            } else {
                children[u]++;
                dfs(v, u);
                low[u] = Math.min(low[u], low[v]);
                
                // If v cannot reach back to u's ancestor, u is an articulation point
                if (low[v] >= tin[u] && p !== -1) {
                    componentsCreated++;
                }
            }
        }
        
        // Root case
        if (p === -1) {
            vulnerability[u] = children[u] > 1 ? children[u] : 0;
        } else {
            // +1 because the part above u stays connected
            vulnerability[u] = componentsCreated > 0 ? componentsCreated + 1 : 0;
        }
    }

    // Handle disconnected graphs
    for (let i = 0; i < n; i++) {
        if (tin[i] === -1) dfs(i);
    }

    let totalSum = 0;
    for (let i = 0; i < n; i++) {
        totalSum += vulnerability[i] * weights[i];
    }

    return totalSum;
}
```

## Complexity
*   **Time:** `O(V + E)`, where `V` is the number of stations and `E` is the number of quantum links. We perform a single Depth First Search traversal of the graph.
*   **Space:** `O(V + E)` to store the adjacency list and the auxiliary arrays (`tin`, `low`, `vulnerability`) used during the DFS traversal.