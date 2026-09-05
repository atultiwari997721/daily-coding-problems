# The Galactic Signal Relay

## Description
You are managing a chain of $N$ space stations arranged in a line, indexed from $0$ to $N-1$. Each station $i$ has a signal strength $S[i]$.

To ensure data integrity, a signal must be relayed through a sequence of stations. A **relay path** is valid if:
1. It consists of a sequence of indices $i_1 < i_2 < \dots < i_k$.
2. For any two adjacent stations in the sequence, the absolute difference in their signal strengths is exactly $D$, i.e., $|S[i_j] - S[i_{j+1}]| = D$.

Given an array of integers `S` and an integer `D`, return the length of the **longest** valid relay path.

## Examples

**Example 1:**
* **Input:** `S = [1, 5, 3, 7, 5, 9]`, `D = 2`
* **Output:** `4`
* **Explanation:** A valid relay path is `[1, 3, 5, 7]` (indices 0, 2, 4, 3? No, indices must be increasing). Wait, valid sequence: `1 -> 3 -> 5 -> 7` (indices 0, 2, 4, 5). Length is 4.

**Example 2:**
* **Input:** `S = [1, 2, 3, 4]`, `D = 1`
* **Output:** `4`
* **Explanation:** The path `1 -> 2 -> 3 -> 4` (indices 0, 1, 2, 3) is valid.

**Example 3:**
* **Input:** `S = [10, 20, 30]`, `D = 5`
* **Output:** `1`
* **Explanation:** No two adjacent elements have a difference of 5. The longest path is just any single station.

## Solution (Python)

```python
def longest_relay_path(S, D):
    if not S:
        return 0
    
    # dp[val] will store the length of the longest relay path 
    # ending with a signal strength of 'val'.
    dp = {}
    max_length = 0
    
    for val in S:
        # Check the two possible signal strengths that could have 
        # preceded this one: (val - D) and (val + D)
        prev1 = val - D
        prev2 = val + D
        
        # Current length is 1 + the best length ending in a valid predecessor
        current_best = 1
        if prev1 in dp:
            current_best = max(current_best, dp[prev1] + 1)
        
        # Special check if D is 0 to avoid double counting 
        # (though the logic holds as dp[val] is updated after check)
        if D != 0 and prev2 in dp:
            current_best = max(current_best, dp[prev2] + 1)
        elif D == 0 and prev1 in dp:
             current_best = max(current_best, dp[prev1] + 1)

        # Update the DP map for this signal strength
        dp[val] = max(dp.get(val, 0), current_best)
        max_length = max(max_length, dp[val])
        
    return max_length
```

## Complexity
Time: **O(N)**, where N is the number of stations, as we iterate through the array once and perform dictionary lookups in O(1).
Space: **O(N)**, to store the `dp` dictionary containing the longest path lengths for each unique signal strength encountered.