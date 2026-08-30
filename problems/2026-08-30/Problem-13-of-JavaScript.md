# The Galactic Relay Signal

## Description
You are tasked with optimizing a communication relay system for a fleet of spaceships. You are given an array of integers `signals`, where each element represents the signal strength received at a specific sector in space.

A **stable sequence** is defined as a contiguous subarray where the absolute difference between any two elements in the subarray is **at most 1**.

Your goal is to find the **length of the longest stable sequence** that can be formed by concatenating **exactly two** stable sequences from the array. Note that these two sequences must be disjoint (they cannot overlap in their original indices).

## Examples

**Example 1:**
Input: `signals = [1, 2, 2, 3, 1, 1]`
Output: `4`
*Explanation:* You can pick the stable sequence `[1, 2, 2]` (length 3) and `[1, 1]` (length 2), but they overlap. Alternatively, you can pick `[1, 2, 2]` (indices 0-2) and `[1, 1]` (indices 4-5). Total length: 3 + 2 = 5? No, the rule is any two *disjoint* stable sequences. The best disjoint pair is `[1, 2, 2]` and `[1, 1]` which is length 5. Wait, actually, let's look at `[2, 2, 3]` and `[1, 1]`. That's 3 + 2 = 5.

**Example 2:**
Input: `signals = [4, 4, 4, 8, 8]`
Output: `5`
*Explanation:* Stable sequences are `[4, 4, 4]` (len 3) and `[8, 8]` (len 2). Total length = 5.

## Solution (JavaScript)

```javascript
/**
 * @param {number[]} signals
 * @return {number}
 */
function longestStableRelay(signals) {
    const n = signals.length;
    if (n === 0) return 0;
    if (n === 1) return 1;

    // Find all maximal stable sequences and store their [start, end, length]
    const stables = [];
    let i = 0;
    while (i < n) {
        let start = i;
        let minVal = signals[i];
        let maxVal = signals[i];
        
        let j = i;
        while (j < n) {
            let nextMin = Math.min(minVal, signals[j]);
            let nextMax = Math.max(maxVal, signals[j]);
            if (nextMax - nextMin <= 1) {
                minVal = nextMin;
                maxVal = nextMax;
                j++;
            } else {
                break;
            }
        }
        stables.push({ start, end: j - 1, len: j - i });
        i = j; // Jump to the start of the next potential sequence
    }

    let maxTotal = 0;

    // Compare every pair of disjoint stable sequences
    for (let a = 0; a < stables.length; a++) {
        for (let b = a + 1; b < stables.length; b++) {
            maxTotal = Math.max(maxTotal, stables[a].len + stables[b].len);
        }
    }

    return maxTotal;
}
```

## Complexity
Time: **O(N + K^2)** where $N$ is the length of the input array and $K$ is the number of maximal stable sequences (in the worst case $K \approx N/2$). 

Space: **O(K)** to store the metadata of the stable sequences found.