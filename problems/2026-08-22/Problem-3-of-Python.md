# Galactic Relay Network Optimization

## Description

You are tasked with optimizing a galactic communication network represented as a set of $N$ planets, labeled $0$ to $N-1$. Some planets are connected by bidirectional quantum relays. Each relay has a **latency cost**.

However, the network is unstable. A "Galactic Storm" can occur at any time, rendering a set of edges unusable. You are given a sequence of $Q$ queries. Each query consists of:
1. A set of edges to **temporarily deactivate**.
2. A destination planet $T$.

For each query, you must calculate the **Shortest Path** from planet $0$ to planet $T$. If $T$ is unreachable, return `-1`. 

**Constraints:**
- $N \le 500$
- Total number of edges $M \le 2000$
- Number of queries $Q \le 100$
- For each query, the number of deactivated edges $D \le 10$
- Latency costs are positive integers.

## Examples

**Example 1:**
Input: 
`N = 4`, `edges = [(0,1,10), (1,2,10), (0,2,25), (2,3,5)]`, `queries = [([ (1,2) ], 3)]`
Output: `[30]`
Explanation: 
Initial path 0->2->3 is 30. Path 0->1->2->3 is 25. Deactivating (1,2) removes the 0->1->2 connection, forcing the path 0->2->3. Total cost 30.

**Example 2:**
Input: 
`N = 3`, `edges = [(0,1,5), (1,2,5)]`, `queries = [([ (0,1) ], 2)]`
Output: `[-1]`
Explanation: 
Deactivating (0,1) disconnects planet 0 from the rest of the network.

## Solution (Python)

```python
import heapq

def solve(N, edges, queries):
    # Build adjacency list
    adj = [[] for _ in range(N)]
    edge_map = {} # Map (u, v) to index
    for i, (u, v, w) in enumerate(edges):
        adj[u].append((v, w, i))
        adj[v].append((u, w, i))
        edge_map[tuple(sorted((u, v)))] = i
    
    results = []
    
    for deactivated_edges, target in queries:
        # Create a set of indices for deactivated edges
        deactivated_indices = set()
        for u, v in deactivated_edges:
            idx = edge_map.get(tuple(sorted((u, v))))
            if idx is not None:
                deactivated_indices.add(idx)
        
        # Dijkstra's Algorithm
        pq = [(0, 0)]
        dist = {0: 0}
        found = -1
        
        while pq:
            d, u = heapq.heappop(pq)
            
            if u == target:
                found = d
                break
            
            if d > dist.get(u, float('inf')):
                continue
                
            for v, w, idx in adj[u]:
                if idx not in deactivated_indices:
                    if d + w < dist.get(v, float('inf')):
                        dist[v] = d + w
                        heapq.heappush(pq, (d + w, v))
                        
        results.append(found)
        
    return results
```

## Complexity
Time: $O(Q \cdot (M + N \log N))$  
Where $Q$ is the number of queries, $M$ is the number of edges, and $N$ is the number of nodes. Since $D$ is small, the overhead of checking deactivated edges is constant per edge traversal.

Space: $O(N + M)$  
To store the adjacency list representation of the graph and the distance dictionary during Dijkstra's execution.