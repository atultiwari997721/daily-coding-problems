# The Synchronized Constellation Grid

## Description

You are observing a 2D grid of size $N \times M$ representing a celestial field. Each cell $(i, j)$ contains a star that emits light according to a specific cycle. A star at $(i, j)$ has a **pulsation period** $P_{i,j}$. At time $t=0$, all stars are dark. A star at $(i, j)$ is "bright" if $t \pmod{P_{i,j}} < K_{i,j}$, where $K_{i,j}$ is its duration of brightness.

You are given a **Synchronization Rule**: You can choose a single time $T$ to view the constellation. However, you can only perform a "Spatial Shift" once: you may choose a vector $(dr, dc)$ and shift the entire grid such that a star originally at $(r, c)$ now occupies $(r+dr, c+dc)$. A star is "active" if it is bright at time $T$ **and** its new position $(r+dr, c+dc)$ satisfies a specific property: the cell must be "aligned" with its original coordinates, meaning $(r+dr) \pmod{R} = r_0$ and $(c+dc) \pmod{C} = c_0$ for some fixed constants $R, C$.

Find the **maximum number of active stars** you can achieve by selecting an optimal time $T$ ($0 \le T < \text{LCM of all } P_{i,j}$) and an optimal shift $(dr, dc)$.

*Note: For the sake of this problem, $N, M \le 100$, and all $P_{i,j}$ are small integers (1 to 10).*

## Examples

**Example 1:**
Input: 
`grid_P = [[2, 3], [2, 2]]`, `grid_K = [[1, 1], [1, 1]]`, `R=2, C=2`, `r0=0, c0=0`
Output: `3`
*Explanation: At T=1, stars (0,0), (0,1), (1,0) are bright. With a shift (0,0), they align with (0,0), (0,1), (1,0) which satisfy the modulo constraints.*

**Example 2:**
Input:
`grid_P = [[2, 2], [2, 2]]`, `grid_K = [[1, 1], [1, 1]]`, `R=1, C=1`, `r0=0, c0=0`
Output: `4`
*Explanation: At T=1, all 4 stars are bright. Any shift works.*

## Solution (Python)

```python
import math
from collections import defaultdict

def max_active_stars(grid_P, grid_K, R, C, r0, c0):
    N = len(grid_P)
    M = len(grid_P[0])
    
    # Precompute brightness per time slot
    # Since P <= 10, LCM is 2520
    LCM = 2520
    
    # brightness[t] stores list of (r, c) that are bright at time t
    bright_at = [[] for _ in range(LCM)]
    for r in range(N):
        for c in range(M):
            for t in range(LCM):
                if t % grid_P[r][c] < grid_K[r][c]:
                    bright_at[t].append((r, c))
                    
    max_stars = 0
    
    # For each time t, we want to find a shift (dr, dc)
    # that maximizes count of (r, c) where:
    # (r + dr) % R == r0  =>  dr % R == (r0 - r) % R
    # (c + dc) % C == c0  =>  dc % C == (c0 - c) % C
    
    for t in range(LCM):
        # Count shifts (dr_mod, dc_mod)
        # where dr_mod = dr % R, dc_mod = dc % C
        shift_counts = defaultdict(int)
        for r, c in bright_at[t]:
            dr_mod = (r0 - r) % R
            dc_mod = (c0 - c) % C
            shift_counts[(dr_mod, dc_mod)] += 1
            
        if shift_counts:
            max_stars = max(max_stars, max(shift_counts.values()))
            
    return max_stars
```

## Complexity
Time: $O(LCM(P) \cdot N \cdot M)$, where $LCM(P)$ is 2520. Given $N, M \le 100$, this is roughly $2.5 \times 10^7$ operations, fitting within time limits.
Space: $O(LCM(P) \cdot N \cdot M)$ to store the precomputed brightness map, though this can be optimized to $O(N \cdot M)$ if calculated on the fly.