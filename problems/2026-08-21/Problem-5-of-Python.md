# The Chronos Relay Network

## Description

You are tasked with managing a network of $N$ temporal relay stations labeled $0$ to $N-1$. Each station $i$ has a **"Chronos Charge"** value $C_i$. 

Communication between two stations $u$ and $v$ is only possible if they are directly connected by a temporal link. You are given a list of $M$ bidirectional links. However, these links have a **"Time-Dilation Factor"** $D_j$. To traverse a link with factor $D_j$, your current "Charge" must be a multiple of $D_j$.

When you arrive at a station $i$ from a link, your Charge is updated: $C_{new} = C_{old} + C_i$. 

Starting at station `start` with an initial Charge $C_{start}$, determine the **minimum number of jumps** required to reach station `end`. If it is impossible to reach the target, return -1.

## Examples

**Example 1:**
*   **Input:** `N = 3`, `links = [[0, 1, 2], [1, 2, 3]]`, `charges = [10, 2, 5]`, `start = 0`, `end = 2`
*   **Process:** 
    1. Start at 0, Charge = 10.
    2. Link to 1: $10$ is divisible by $2$ (link $D=2$). New charge: $10 + 2 = 12$.
    3. Link to 2: $12$ is divisible by $3$ (link $D=3$). New charge: $12 + 5 = 17$.
*   **Output:** `2`

**Example 2:**
*   **Input:** `N = 2`, `links = [[0, 1, 5]]`, `charges = [10, 2]`, `start = 0`, `end = 1`
*   **Process:** Start 0 (Charge 10). Link to 1 requires $10 \% 5 == 0$. New charge $10 + 2 = 12$.
*   **Output:** `1`

## Solution (Python)

To solve this, we use Dijkstra's algorithm. Because the charge can potentially grow, we must track states as `(charge, node)`. Since we want the minimum jumps, we prioritize states based on the number of jumps made.

```python
import heapq

def min_jumps(N, links, charges, start, end):
    # adjacency list: graph[u] = [(v, dilation_factor), ...]
    graph = [[] for _ in range(N)]
    for u, v, d in links:
        graph[u].append((v, d))
        graph[v].append((u, d))
    
    # pq stores (jumps, current_charge, current_node)
    # We use a set to keep track of visited states to avoid cycles
    # State: (current_node, current_charge)
    pq = [(0, charges[start], start)]
    visited = {} 
    
    while pq:
        jumps, curr_charge, u = heapq.heappop(pq)
        
        if u == end:
            return jumps
        
        if visited.get((u, curr_charge), float('inf')) <= jumps:
            continue
        visited[(u, curr_charge)] = jumps
        
        for v, d in graph[u]:
            if curr_charge % d == 0:
                new_charge = curr_charge + charges[v]
                heapq.heappush(pq, (jumps + 1, new_charge, v))
                
    return -1
```

## Complexity

*   **Time:** $O(E \cdot \text{states} \cdot \log(\text{states}))$, where states are the unique (node, charge) pairs reachable. In the worst case, this depends on the growth of the charge, but given the structure, it functions like a BFS on an expanded state graph.
*   **Space:** $O(V \cdot \text{unique\_charges})$, to store the visited states and the priority queue.