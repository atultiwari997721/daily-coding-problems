# The Galactic Relay Network

## Description

You are managing a constellation of $N$ satellites in orbit, indexed from $0$ to $N-1$. Some satellites are directly connected via bidirectional communication links. 

Due to atmospheric interference, each link $(u, v)$ has a **signal decay factor** $w$ (where $0 < w \le 1$). When a signal is transmitted along a path, the total signal strength is the **product** of all the decay factors along that path.

A relay station is considered "Stable" if the signal strength of the **strongest possible path** from a base station (satellite $0$) to that satellite is at least a threshold value $T$.

However, there is a catch: you can perform an **"Orbital Boost"** on exactly one satellite (excluding the base station). Boosting a satellite multiplies the decay factors of all *outgoing* edges connected to it by a constant factor $B > 1$. If a decay factor exceeds $1.0$ after the boost, it is capped at $1.0$.

Given an adjacency list of satellites, the signal decay factors, a threshold $T$, and a boost factor $B$, find the maximum number of satellites (including the base station) that can become "Stable" after choosing the optimal satellite to boost.

## Examples

**Example 1:**
*   **Input:** `N = 3`, `edges = [[0,1,0.5], [1,2,0.8]]`, `T = 0.4`, `B = 2.0`
*   **Output:** `3`
*   **Explanation:** Base station $0$ has strength $1.0$ (Stable). Satellite $1$ has $0.5$ (Stable). Satellite $2$ has $0.4$ (Stable). Boosting $1$ makes edge $(1,2)$ become $\min(1, 0.8 * 2.0) = 1.0$.

**Example 2:**
*   **Input:** `N = 3`, `edges = [[0,1,0.1], [1,2,0.1]]`, `T = 0.2`, `B = 10`
*   **Output:** `2`
*   **Explanation:** Without boost, only $0$ is stable. Boosting $1$ makes edge $(0,1)$ become $1.0$, making $1$ stable (strength $1.0$), but $2$ remains $0.1$.

## Solution (JavaScript)

```javascript
/**
 * @param {number} N
 * @param {number[][]} edges
 * @param {number} T
 * @param {number} B
 * @return {number}
 */
function maxStableSatellites(N, edges, T, B) {
    const adj = Array.from({ length: N }, () => []);
    for (const [u, v, w] of edges) {
        adj[u].push({ to: v, w });
        adj[v].push({ to: u, w });
    }

    // Function to get max path strengths using Dijkstra (multiplicative)
    const getStrengths = (boostNode) => {
        const strengths = new Array(N).fill(0);
        strengths[0] = 1.0;
        const pq = [[1.0, 0]]; // [strength, node]

        while (pq.length > 0) {
            pq.sort((a, b) => b[0] - a[0]);
            const [currS, u] = pq.shift();

            if (currS < strengths[u]) continue;

            for (const edge of adj[u]) {
                let weight = edge.w;
                // Apply boost if the edge originates from the boosted node
                if (u === boostNode) {
                    weight = Math.min(1.0, weight * B);
                }
                
                const nextS = currS * weight;
                if (nextS > strengths[edge.to]) {
                    strengths[edge.to] = nextS;
                    pq.push([nextS, edge.to]);
                }
            }
        }
        return strengths;
    };

    let maxStable = 0;
    // Try boosting each node (1 to N-1) or no boost (-1)
    for (let b = -1; b < N; b++) {
        const strengths = getStrengths(b);
        const count = strengths.filter(s => s >= T).length;
        maxStable = Math.max(maxStable, count);
    }

    return maxStable;
}
```

## Complexity
Time: **O(N * (E log V))**, where $N$ is the number of satellites, $E$ is the number of edges, and $V$ is vertices. We run Dijkstra for each potential boosted satellite.

Space: **O(N + E)** to store the graph adjacency list and the strength tracking array.