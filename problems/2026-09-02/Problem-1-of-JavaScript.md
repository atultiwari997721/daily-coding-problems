# The Library Shelf Balancer

## Description
You are organizing a library shelf. You are given an array of integers `books`, where each integer represents the thickness of a book in centimeters. You are also given an integer `k`, representing the number of "shelf segments."

A shelf segment can hold any number of adjacent books. The "clutter level" of a segment is defined as the **total thickness** of the books in that segment. 

Your goal is to divide the `books` array into exactly `k` **non-empty** contiguous segments such that the **maximum** clutter level among all segments is minimized. Return this minimum possible value.

*Note: You must use all books, and each segment must contain at least one book.*

## Examples

**Example 1:**
*   **Input:** `books = [1, 2, 3, 4, 5]`, `k = 2`
*   **Output:** `9`
*   **Explanation:** You can split it as `[1, 2, 3]` (sum 6) and `[4, 5]` (sum 9). Max is 9. Any other split like `[1, 2]` and `[3, 4, 5]` results in 12.

**Example 2:**
*   **Input:** `books = [10, 10, 10, 10]`, `k = 2`
*   **Output:** `20`
*   **Explanation:** Split as `[10, 10]` and `[10, 10]`.

## Solution (JavaScript)

```javascript
/**
 * @param {number[]} books
 * @param {number} k
 * @return {number}
 */
function minimizeMaxClutter(books, k) {
    let low = Math.max(...books);
    let high = books.reduce((a, b) => a + b, 0);
    
    const canSplit = (maxAllowed) => {
        let segments = 1;
        let currentSum = 0;
        
        for (let book of books) {
            if (currentSum + book > maxAllowed) {
                segments++;
                currentSum = book;
            } else {
                currentSum += book;
            }
        }
        return segments <= k;
    };
    
    while (low < high) {
        let mid = Math.floor((low + high) / 2);
        if (canSplit(mid)) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }
    
    return low;
}
```

## Complexity
Time: O(N log(S)), where N is the number of books and S is the sum of all book thicknesses.
Space: O(1), as we only use a few variables to track our bounds and sum.