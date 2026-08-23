# The Last Standing Synchronized Server

## Description
You are managing a distributed system where `n` servers are arranged in a circle, labeled from `1` to `n`. Every `k`-th server is taken offline for maintenance in a repeating loop until only one server remains. However, there is a catch: the interval `k` is not constant. 

Starting with the first server, you remove the `k`-th server. Then, for the next round, the interval `k` increases by 1 (i.e., you remove the `(k+1)`-th server from the *remaining* pool). This continues, with the interval increasing by 1 after every removal, until only one server is left.

Given `n` (number of servers) and the starting interval `k`, return the label of the last standing server.

## Examples

**Example 1:**
* **Input:** `n = 5, k = 2`
* **Process:**
  1. Servers: `[1, 2, 3, 4, 5]`, remove index `(2-1) = 1` (Server 2). Remaining: `[1, 3, 4, 5]`
  2. Servers: `[1, 3, 4, 5]`, next `k=3`. Remove index `(3-1) % 4 = 2` (Server 4). Remaining: `[1, 3, 5]`
  3. Servers: `[1, 3, 5]`, next `k=4`. Remove index `(4-1) % 3 = 0` (Server 1). Remaining: `[3, 5]`
  4. Servers: `[3, 5]`, next `k=5`. Remove index `(5-1) % 2 = 0` (Server 3). Remaining: `[5]`
* **Output:** `5`

**Example 2:**
* **Input:** `n = 3, k = 1`
* **Output:** `3`

## Solution (JavaScript)

```javascript
/**
 * @param {number} n
 * @param {number} k
 * @return {number}
 */
function lastStandingServer(n, k) {
  // We solve this using the Josephus problem approach.
  // The position of the survivor in a circle of size 'i'
  // can be derived from the survivor of size 'i-1'.
  
  // Base case: If there is 1 server, it is at index 0 (0-indexed)
  let survivor = 0;
  
  // We build up the survivor position from i = 2 to n.
  // At each step 'i', the interval is (k + (n - i))
  for (let i = 2; i <= n; i++) {
    // Current interval is k + (n - i) because the interval 
    // increases by 1 each round, and we perform n-1 rounds.
    // The total offset added is the number of steps taken.
    let currentK = k + (n - i);
    survivor = (survivor + currentK) % i;
  }
  
  // Convert 0-indexed result to 1-indexed label
  return survivor + 1;
}
```

## Complexity
Time: O(n) where n is the number of servers, as we iterate from 2 to `n`.
Space: O(1) as we only use a single variable to track the survivor's position.