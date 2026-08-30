# The Galactic Signal Relay

## Description
You are managing a sequence of $N$ interstellar signal relays arranged in a line, labeled $0$ to $N-1$. Each relay $i$ has an associated **Signal Noise** value, $A[i]$.

A "Sub-Relay Network" is defined as a contiguous subarray $[i, j]$. The **Network Stability** of this subarray is calculated as:
$$\text{Stability} = (\text{Sum of elements in } [i, j]) \times (\text{Minimum element in } [i, j])$$

Due to quantum interference, you are tasked with finding the **maximum Network Stability** possible for any contiguous subarray within the given range $[0, N-1]$.

Because the signals are massive, if the stability exceeds $10^9 + 7$, you must return the result **modulo $10^9 + 7$**.

## Examples

**Example 1:**
*   **Input:** `A = [3, 1, 6, 4, 5, 2]`
*   **Output:** `60`
*   **Explanation:** The subarray `[6, 4, 5]` has a sum of 15 and a minimum of 4. $15 \times 4 = 60$.

**Example 2:**
*   **Input:** `A = [1, 2, 3, 4, 5]`
*   **Output:** `25`
*   **Explanation:** The subarray `[1, 2, 3, 4, 5]` has sum 15 (min 1 -> 15), but `[5]` has sum 5 (min 5 -> 25). $25$ is the max.

## Solution (JavaScript)

To solve this optimally, we use the **Monotonic Stack** technique to find the boundaries where each element $A[i]$ acts as the minimum for a subarray.

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
function maxNetworkStability(nums) {
    const n = nums.length;
    const MOD = 1000000007n;
    
    // Calculate prefix sums to get subarray sums in O(1)
    const prefixSum = new Array(n + 1).fill(0n);
    for (let i = 0; i < n; i++) {
        prefixSum[i + 1] = prefixSum[i] + BigInt(nums[i]);
    }
    
    // Find the nearest index to the left that is smaller than nums[i]
    const left = new Array(n).fill(-1);
    const stack = [];
    for (let i = 0; i < n; i++) {
        while (stack.length && nums[stack[stack.length - 1]] >= nums[i]) {
            stack.pop();
        }
        if (stack.length) left[i] = stack[stack.length - 1];
        stack.push(i);
    }
    
    // Find the nearest index to the right that is smaller than nums[i]
    const right = new Array(n).fill(n);
    stack.length = 0;
    for (let i = n - 1; i >= 0; i--) {
        while (stack.length && nums[stack[stack.length - 1]] >= nums[i]) {
            stack.pop();
        }
        if (stack.length) right[i] = stack[stack.length - 1];
        stack.push(i);
    }
    
    let maxStability = 0n;
    
    for (let i = 0; i < n; i++) {
        // The subarray where nums[i] is the minimum is [left[i] + 1, right[i] - 1]
        const sum = prefixSum[right[i]] - prefixSum[left[i] + 1];
        const stability = sum * BigInt(nums[i]);
        
        if (stability > maxStability) {
            maxStability = stability;
        }
    }
    
    return Number(maxStability % MOD);
}
```

## Complexity
Time: **O(N)** — We traverse the array a constant number of times (Prefix sum: O(N), Monotonic stacks: O(N), Final iteration: O(N)).
Space: **O(N)** — Used for the prefix sum array, the two monotonic stacks, and the left/right boundary arrays.