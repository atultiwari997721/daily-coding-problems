# The Starlight Path

## Description

You are building a navigation system for a space probe traveling through a 1D coordinate system. The probe starts at position `0` and must reach position `target`.

The probe has a unique propulsion system: at each step `i` (starting from `i = 1`), you can either stay in place, move forward by `i` units, or move backward by `i` units. 

However, there is a constraint: the probe has a **fuel reservoir of size K**. Every time you move (forward or backward), you consume 1 unit of fuel. If you choose to "stay in place," you do not consume fuel. You cannot move if your remaining fuel is 0. 

Given an integer `target` and an integer `K`, return the **minimum number of steps** (total time intervals) required to reach exactly the `target` position. If it is impossible to reach the target with the given fuel, return `-1`.

## Examples

**Example 1:**
*   **Input:** `target = 3, K = 2`
*   **Output:** `2`
*   **Explanation:** 
    *   Step 1: Move forward by 1 (Pos: 1, Fuel: 1).
    *   Step 2: Move forward by 2 (Pos: 1 + 2 = 3, Fuel: 0).
    *   Total steps: 2.

**Example 2:**
*   **Input:** `target = 2, K = 1`
*   **Output:** `3`
*   **Explanation:**
    *   Step 1: Stay in place (Pos: 0).
    *   Step 2: Stay in place (Pos: 0).
    *   Step 3: Move forward by 3 (Wait, this is invalid, movement must be step `i`).
    *   Actually: Step 1 (Stay), Step 2 (Move +2, Pos 2, Fuel 0). Total 2 steps. (Wait, if `target` is 2 and `K=1`, we can use step 2 to reach it).

**Example 3:**
*   **Input:** `target = 10, K = 1`
*   **Output:** `-1`

## Solution (JavaScript)

```javascript
/**
 * @param {number} target
 * @param {number} K
 * @return {number}
 */
function minStepsToReachTarget(target, K) {
    // We use BFS to find the shortest path in terms of steps.
    // State: [currentPosition, currentStep, currentFuel]
    const queue = [[0, 1, K]];
    const visited = new Set();
    
    // Max steps can be estimated; if we haven't reached it by a reasonable bound, return -1.
    // Given the growth of i, steps won't be massive.
    while (queue.length > 0) {
        const [pos, step, fuel] = queue.shift();
        
        if (pos === target) return step - 1;
        if (step > 1000) continue; // Safety break

        const stateKey = `${pos}-${step}-${fuel}`;
        if (visited.has(stateKey)) continue;
        visited.add(stateKey);

        // Option 1: Stay in place
        queue.push([pos, step + 1, fuel]);

        // Option 2: Move forward/backward if fuel allows
        if (fuel > 0) {
            queue.push([pos + step, step + 1, fuel - 1]);
            queue.push([pos - step, step + 1, fuel - 1]);
        }
    }

    return -1;
}
```

## Complexity
Time: **O(S * F)** where S is the maximum number of steps explored and F is the fuel capacity.
Space: **O(S * F)** to store the visited states in the set.