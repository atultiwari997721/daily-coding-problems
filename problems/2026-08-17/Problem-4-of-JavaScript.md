# The Harmonic Data Stream

## Description

You are given a stream of $N$ integers representing values at different time intervals. A "Harmonic Subsequence" is defined as a non-empty subsequence where the **absolute difference between any two adjacent elements is at most $K$**.

However, there is a twist: you can perform **at most one** "jump" operation. A jump allows you to remove any contiguous sub-segment of the original array (of any length from $0$ to $N-1$) and effectively "stitch" the remaining parts together.

Given an array `nums` and an integer `K`, find the length of the **longest Harmonic Subsequence** you can form after performing at most one jump.

*Note: You are looking for the longest subsequence that satisfies the $|a_i - a_{i+1}| \le K$ property after the stitching operation.*

## Examples

**Example 1:**
Input: `nums = [1, 5, 2, 8, 3], K = 3`
Output: `4`
*Explanation: You can remove the element `8`. The remaining subsequence `[1, 5, 2, 3]` is not valid, but if you remove the index containing `8`, you get `[1, 5, 2, 3]`. Wait, the longest valid one is `[1, 2, 3]` (length 3). If we remove the sub-segment `[5, 8]`, we get `[1, 2, 3]`. Actually, the longest is `[1, 2, 3]` or `[5, 2, 3]`? No, `[1, 2, 3]` is length 3. If we keep `[1, 2, 3]`, that works.*

**Example 2:**
Input: `nums = [10, 12, 11, 20, 22, 21], K = 1`
Output: `6`
*Explanation: The entire array is already a harmonic subsequence because every adjacent difference is $\le 1$.*

## Solution (JavaScript)

```javascript
/**
 * @param {number[]} nums
 * @param {number} K
 * @return {number}
 */
function longestHarmonicSubsequence(nums, K) {
    const n = nums.length;
    if (n === 0) return 0;

    // A subsequence must maintain relative order.
    // The "jump" operation allows us to pick a subsequence from:
    // nums[0...i] + nums[j...n-1] where j > i.
    // This effectively means we are looking for the longest chain
    // across the two ends of the array.
    
    function getLongestHarmonic(arr) {
        if (arr.length === 0) return 0;
        let dp = new Array(arr.length).fill(1);
        for (let i = 1; i < arr.length; i++) {
            if (Math.abs(arr[i] - arr[i - 1]) <= K) {
                dp[i] = dp[i - 1] + 1;
            }
        }
        return Math.max(...dp);
    }

    let maxLen = getLongestHarmonic(nums);

    // Try all possible jump points (split index)
    // We split into nums[0...i] and nums[j...n-1]
    // Since we can remove any contiguous segment, we are effectively 
    // joining two prefixes/suffixes or just taking a prefix or suffix.
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j <= n; j++) {
            const combined = [...nums.slice(0, i), ...nums.slice(j)];
            maxLen = Math.max(maxLen, getLongestHarmonic(combined));
        }
    }

    return maxLen;
}

// Note: The brute force above is O(N^3). 
// An optimized version uses Dynamic Programming to track 
// harmonic chains from the left and right.
```

## Complexity
Time: O(N^3) for the provided snippet; the optimal dynamic programming approach is O(N^2) by precomputing harmonic chains from both ends and merging them.
Space: O(N) to store the chain lengths.