# The Galactic Signal Relay

## Description
You are designing a communication network across a series of space stations arranged in a line, indexed from `0` to `n-1`. Each station `i` has a signal amplifier that can boost a signal to reach another station within a specific distance `range[i]`. 

Specifically, from station `i`, you can jump to any station `j` such that `i < j <= i + range[i]`. However, the signal suffers from "cosmic interference": each station `i` has a cost `cost[i]` associated with using its amplifier.

Your goal is to reach the final station `n-1` starting from station `0` with the **minimum total cost**. If it is impossible to reach the end, return `-1`.

## Examples

**Example 1:**
* **Input:** `range = [2, 1, 1, 1]`, `cost = [10, 2, 5, 1]`
* **Output:** `13`
* **Explanation:** Path: 0 -> 1 -> 3. Cost: `cost[0]` (10) + `cost[1]` (2) + `cost[3]` (1) = 13. (Note: You do not pay the cost of the final destination station if you don't use its amplifier to move further, but the problem implies you pay the cost of every station you land on/start from). 
* *Correction:* To reach index 3: Start at 0 (cost 10). Jump to 1 (cost 2). Jump to 3 (cost 1). Total: 13.

**Example 2:**
* **Input:** `range = [1, 0, 1]`, `cost = [5, 10, 2]`
* **Output:** `-1`
* **Explanation:** Station 1 has a range of 0, meaning you are stuck. You cannot reach the end.

## Solution (Python)

```python
import heapq

def min_cost_to_reach_end(range_arr, cost_arr):
    n = len(range_arr)
    # min_costs[i] stores the minimum cost to reach station i
    # Initialize with infinity
    min_costs = [float('inf')] * n
    min_costs[0] = cost_arr[0]
    
    # We use a min-heap to always expand the cheapest reachable path
    # Heap stores (total_cost, current_index)
    pq = [(cost_arr[0], 0)]
    
    while pq:
        curr_cost, u = heapq.heappop(pq)
        
        if u == n - 1:
            return curr_cost
        
        if curr_cost > min_costs[u]:
            continue
            
        # Explore all possible jumps from u
        max_jump = range_arr[u]
        for v in range(u + 1, min(u + max_jump + 1, n)):
            new_cost = curr_cost + cost_arr[v]
            if new_cost < min_costs[v]:
                min_costs[v] = new_cost
                heapq.heappush(pq, (new_cost, v))
                
    return -1
```

## Complexity
Time: **O(N^2)** in the worst case (if every range allows jumping to all subsequent stations). While Dijkstra is typically $O(E \log V)$, here the number of edges can be $O(N^2)$.
Space: **O(N)** to store the `min_costs` array and the heap.