# The Synchronized Server Pulse

## Description

You are managing a distributed system where `n` servers are arranged in a circular formation (labeled `0` to `n-1`). Each server `i` has a `pulseValue[i]`. 

A signal starts at `server 0` and jumps to other servers based on the current server's `pulseValue`. Specifically, if you are at `server i` with value `v = pulseValue[i]`, you must jump to `(i + v) % n`. 

The system enters a "feedback loop" if you ever visit a server you have already visited. Your task is to determine if the signal **eventually visits every single server** in the network before a loop is formed or if it gets stuck in a cycle early.

Return `true` if all servers are visited, otherwise return `false`.

## Examples

**Example 1:**
* **Input:** `pulseValue = [2, 3, 1, 1]`
* **Output:** `true`
* **Explanation:** 
  - Start at 0: value 2, jump to 2.
  - At 2: value 1, jump to 3.
  - At 3: value 1, jump to 0 (Loop detected!).
  - Wait, let's trace: 0 -> 2 -> 3 -> 0. Only visited {0, 2, 3}. Missing server 1. Output: `false`.

**Example 2:**
* **Input:** `pulseValue = [1, 1, 1]`
* **Output:** `true`
* **Explanation:**
  - Start at 0: jump to 1.
  - At 1: jump to 2.
  - At 2: jump to 0. All servers {0, 1, 2} visited. Output: `true`.

## Solution (JavaScript)

```javascript
/**
 * @param {number[]} pulseValue
 * @return {boolean}
 */
function canVisitAllServers(pulseValue) {
    const n = pulseValue.length;
    const visited = new Set();
    let currentIdx = 0;
    
    // We can visit at most n servers before we must repeat
    // or finish the full traversal.
    while (!visited.has(currentIdx)) {
        visited.add(currentIdx);
        
        // Calculate the next jump
        let jump = pulseValue[currentIdx];
        currentIdx = (currentIdx + jump) % n;
        
        // If we have visited all servers, we are done
        if (visited.size === n) {
            return true;
        }
    }
    
    // If we broke the loop and size < n, we failed to visit all
    return false;
}
```

## Complexity
Time: **O(n)**, where `n` is the number of servers. We visit each server at most once before the loop condition triggers or we complete the traversal.

Space: **O(n)** to maintain the `Set` of visited servers.