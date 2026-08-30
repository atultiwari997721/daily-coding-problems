# The Galactic Relay Network

## Description

You are managing a Galactic Relay Network consisting of $N$ planets, indexed $0$ to $N-1$. Communication between planets is handled by quantum relays. You are given a list of $M$ potential relay connections, where each connection `[u, v, w]` represents a bidirectional link between planet $u$ and planet $v$ with a signal latency $w$.

Due to extreme stellar radiation, **every planet $i$ has a maximum relay capacity $C_i$**. A relay connection $(u, v)$ can only be active if the total number of active connections connected to $u$ does not exceed $C_u$ AND the total number of active connections connected to $v$ does not exceed $C_v$.

Your goal is to select a subset of the $M$ connections to form a **connected** network (a spanning tree or subgraph that links all $N$ planets) such that the **sum of latencies of the selected connections is minimized**, subject to the capacity constraints $C_i$. If it is impossible to connect all planets, return -1.

## Examples

**Example 1:**
*   **Input:** 
    `N = 3`, `edges = [[0, 1, 5], [1, 2, 10], [0, 2, 2]]`, `capacities = [1, 1, 1]`
*   **Output:** `-1`
*   **Explanation:** To connect 3 nodes, you need at least 2 edges. However, the capacities are all 1, meaning no node can have more than one connection. It is impossible to form a connected graph.

**Example 2:**
*   **Input:** 
    `N = 3`, `edges = [[0, 1, 5], [1, 2, 10], [0, 2, 2]]`, `capacities = [2, 2, 2]`
*   **Output:** `7`
*   **Explanation:** We pick edges `(0, 2)` with weight 2 and `(0, 1)` with weight 5. Total latency = 7. Node 0 degree is 2, Nodes 1 and 2 degrees are 1. All within capacity 2.

## Solution (Python)

This problem is a variation of the Minimum Spanning Tree (MST) problem with degree constraints. Since the degree constraint makes the general problem NP-Hard, for this specific "Hard" variant, we use a backtracking approach with pruning (or a modified Prim's algorithm if constraints allow), but given the small constraints usually associated with this type of problem, a **Held-Karp style bitmask DP or state-space search** is the standard approach.

```python
import heapq

def solve(N, edges, capacities):
    # Sort edges by weight to attempt a greedy-like exploration
    edges.sort(key=lambda x: x[2])
    min_total_latency = float('inf')
    
    # State: (current_index, current_degrees, current_latency, connected_components)
    # Using Union-Find to track connectivity
    def find(parent, i):
        if parent[i] == i: return i
        return find(parent, parent[i])

    def backtrack(idx, degrees, latency, parent, edges_count):
        nonlocal min_total_latency
        
        # Base case: All nodes connected
        if edges_count == N - 1:
            if len(set(find(parent, i) for i in range(N))) == 1:
                min_total_latency = min(min_total_latency, latency)
            return

        if idx >= len(edges) or latency >= min_total_latency:
            return

        u, v, w = edges[idx]
        
        # Option 1: Include this edge if capacity allows
        if degrees[u] < capacities[u] and degrees[v] < capacities[v]:
            root_u, root_v = find(parent, u), find(parent, v)
            if root_u != root_v:
                # Apply
                old_parent = parent[:]
                parent[root_u] = root_v
                degrees[u] += 1
                degrees[v] += 1
                
                backtrack(idx + 1, degrees, latency + w, parent, edges_count + 1)
                
                # Backtrack
                parent[:] = old_parent
                degrees[u] -= 1
                degrees[v] -= 1
        
        # Option 2: Skip this edge
        backtrack(idx + 1, degrees, latency, parent, edges_count)

    backtrack(0, [0]*N, 0, list(range(N)), 0)
    return min_total_latency if min_total_latency != float('inf') else -1
```

## Complexity
Time: **O(2^M)** in the worst case (where M is the number of edges), due to the branching nature of the decision tree.
Space: **O(N + M)** to store the recursion stack, the parent array for Union-Find, and the edge list.