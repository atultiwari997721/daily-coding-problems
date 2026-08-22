# The Quantum Processor Scheduler

## Description

You are designing a scheduler for a quantum processor that executes tasks on a set of $N$ superconducting qubits. Because of the nature of quantum entanglement, the processor can only execute tasks in **"Synchronous Interference Batches"**.

You are given an array of tasks, where `tasks[i] = [start_time, end_time, weight]`. 
However, there is a constraint: a batch can only contain tasks that **do not overlap**. Furthermore, due to the cooling system, any two tasks $A$ and $B$ in the same batch must satisfy the "Coherence Gap": the `end_time` of task $A$ and the `start_time` of task $B$ must differ by at least $K$ units of time (i.e., $|A.end - B.start| \ge K$).

Your goal is to find the **maximum total weight** of a single batch of tasks that can be executed on the processor.

## Examples

**Example 1:**
*   **Input:** `tasks = [[1, 3, 5], [2, 4, 6], [5, 8, 10]]`, `K = 2`
*   **Output:** `15`
*   **Explanation:** You can pick `[1, 3, 5]` and `[5, 8, 10]`. The gap between `3` and `5` is `2`, which satisfies the requirement ($\ge 2$). $5 + 10 = 15$. Note: `[2, 4, 6]` overlaps with the first task.

**Example 2:**
*   **Input:** `tasks = [[1, 2, 10], [3, 4, 10], [5, 6, 10]]`, `K = 1`
*   **Output:** `30`
*   **Explanation:** All tasks can be picked as they are separated by at least 1 unit of time.

## Solution (JavaScript)

```javascript
/**
 * @param {number[][]} tasks
 * @param {number} K
 * @return {number}
 */
function maxQuantumWeight(tasks, K) {
    // Sort tasks by end_time to use dynamic programming
    tasks.sort((a, b) => a[1] - b[1]);

    const n = tasks.length;
    // dp[i] stores the max weight possible considering tasks up to index i
    const dp = new Array(n).fill(0);

    for (let i = 0; i < n; i++) {
        const [start, end, weight] = tasks[i];
        dp[i] = weight;

        // Find the most recent task j such that tasks[j].end + K <= tasks[i].start
        // We use binary search to find the rightmost index
        let low = 0, high = i - 1;
        let bestPrev = -1;

        while (low <= high) {
            let mid = Math.floor((low + high) / 2);
            if (tasks[mid][1] + K <= start) {
                bestPrev = mid;
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }

        if (bestPrev !== -1) {
            dp[i] += dp[bestPrev];
        }

        // Ensure dp[i] is at least the max weight found so far
        if (i > 0) {
            dp[i] = Math.max(dp[i], dp[i - 1]);
        }
    }

    return dp[n - 1];
}
```

## Complexity
Time: **O(N log N)** where N is the number of tasks. Sorting takes O(N log N) and the binary search within the loop takes O(N log N).

Space: **O(N)** to store the dynamic programming array.