# The Library Shelf Balancer

## Description
You are given an array of integers `books`, where each integer represents the thickness of a book in centimeters. You are also given an integer `k`, representing the number of "shelf segments" you have. 

To maintain balance, a shelf is considered "stable" if the sum of book thicknesses on that shelf is **even**. Your goal is to determine if it is possible to partition the `books` into exactly `k` non-empty contiguous segments such that the sum of each segment is even.

Return `true` if such a partition exists, otherwise return `false`.

## Examples

**Example 1:**
*   **Input:** `books = [2, 4, 6, 8]`, `k = 2`
*   **Output:** `true`
*   **Explanation:** We can partition them into `[2, 4]` (sum 6) and `[6, 8]` (sum 14). Both are even.

**Example 2:**
*   **Input:** `books = [1, 3, 5, 7]`, `k = 2`
*   **Output:** `false`
*   **Explanation:** Any partition will result in odd-sum segments because the total sum is 16 (even), but individual segments fail to satisfy the parity requirement.

**Example 3:**
*   **Input:** `books = [2, 2, 2, 2, 2]`, `k = 3`
*   **Output:** `false`
*   **Explanation:** We cannot split 5 books into 3 segments where every segment sum is even.

## Solution (JavaScript)
```javascript
/**
 * @param {number[]} books
 * @param {number} k
 * @return {boolean}
 */
function canBalanceShelves(books, k) {
    // A segment sum is even if it contains an even number of odd numbers.
    // Let's count how many segments we can form that have an even sum.
    let evenSegments = 0;
    let currentSum = 0;

    for (let i = 0; i < books.length; i++) {
        currentSum += books[i];
        
        // If the current accumulated sum is even, we have found a potential segment.
        // We ensure we aren't at the very end unless k segments are completed.
        if (currentSum % 2 === 0) {
            evenSegments++;
            currentSum = 0; // Reset for the next segment
        }
    }

    // We need exactly k segments. If we found more, we can merge 
    // the excess into the last segment because (Even + Even = Even).
    // However, if we found less than k, it's impossible.
    return evenSegments >= k && currentSum === 0;
}
```

## Complexity
Time: O(n), where n is the number of books, as we iterate through the array once.
Space: O(1), as we only use a few variables to track the state.