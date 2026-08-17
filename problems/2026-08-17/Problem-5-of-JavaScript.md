# The Archival Shuffler

## Description
You are given an array of integers `docs` representing a sequence of document IDs and an integer `k`. 

Due to a faulty archival system, every **contiguous block of `k` documents** in the original sequence has been "mirrored" (reversed). However, because of the system's logic, these blocks overlap: the first block is `[0, k-1]`, the second block starts at index `1` and ends at index `k`, the third at `2` to `k+1`, and so on, until the end of the array is reached.

Given the array `docs` that has already undergone this transformation, your task is to **reverse the process** to recover the original sequence of document IDs.

*Note: You may assume the transformation was applied sequentially from left to right.*

## Examples

**Example 1:**
* **Input:** `docs = [3, 2, 1, 4]`, `k = 3`
* **Output:** `[1, 2, 3, 4]`
* **Explanation:** 
    1. Start: `[1, 2, 3, 4]`
    2. Reverse first 3: `[3, 2, 1, 4]`
    3. Next block (indices 1-3) reverse: `[3, 4, 1, 2]` ... wait, the problem asks to reverse the transformation. To undo the effect, we apply the operations in reverse order.

**Example 2:**
* **Input:** `docs = [5, 4, 3, 2, 1]`, `k = 2`
* **Output:** `[1, 2, 3, 4, 5]`

## Solution (JavaScript)

To reverse the transformation, we must perform the reversal operations in the **exact opposite order** of how they were applied. If the forward process applied reversals starting from index `0` up to `n-k`, we must apply the reversals starting from `n-k` down to `0`.

```javascript
/**
 * @param {number[]} docs
 * @param {number} k
 * @return {number[]}
 */
function recoverDocuments(docs, k) {
    const n = docs.length;
    const result = [...docs];

    // The forward transformation reversed windows starting at 0, 1, ..., n-k.
    // To undo this, we reverse the windows starting at n-k, n-k-1, ..., 0.
    for (let i = n - k; i >= 0; i--) {
        reverseWindow(result, i, i + k - 1);
    }

    return result;
}

function reverseWindow(arr, start, end) {
    while (start < end) {
        [arr[start], arr[end]] = [arr[end], arr[start]];
        start++;
        end--;
    }
}
```

## Complexity

*   **Time:** `O(n * k)`, where `n` is the length of the array. We iterate through `n-k` windows, and each reversal takes `O(k)` time.
*   **Space:** `O(n)` to store the result array (or `O(1)` if we perform the operations in-place on the input array).