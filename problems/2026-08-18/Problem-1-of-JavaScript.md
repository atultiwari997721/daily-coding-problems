# The Lost Library Key

## Description
You are a librarian organizing a row of books on a single shelf. You are given an array of integers `books` where each integer represents a book's unique ID. Some books are labeled with a "Key ID" (represented by a value of `0`).

A shelf is considered **"Unlocked"** if there is at least one Key ID (`0`) present. However, you want to make sure the library is efficient: if there is a Key ID, you want to know the **minimum distance** (number of books) between the very first Key ID and the very last Key ID appearing on the shelf. If there is only one Key ID or no Key ID at all, the distance is `0`.

Given an array of integers `books`, return the distance between the first and last occurrence of `0`.

## Examples

**Example 1:**
Input: `books = [1, 0, 5, 2, 0, 3]`
Output: `3`
*Explanation: The first 0 is at index 1 and the last 0 is at index 4. The distance is 4 - 1 = 3.*

**Example 2:**
Input: `books = [0, 8, 9, 0, 7, 0]`
Output: `5`
*Explanation: The first 0 is at index 0 and the last 0 is at index 5. The distance is 5 - 0 = 5.*

**Example 3:**
Input: `books = [1, 2, 3]`
Output: `0`
*Explanation: There are no Key IDs present.*

## Solution (JavaScript)
```javascript
/**
 * @param {number[]} books
 * @return {number}
 */
function minDistanceBetweenKeys(books) {
    let firstIndex = -1;
    let lastIndex = -1;

    for (let i = 0; i < books.length; i++) {
        if (books[i] === 0) {
            if (firstIndex === -1) {
                firstIndex = i;
            }
            lastIndex = i;
        }
    }

    // If no 0s were found or only one 0 exists, distance is 0
    if (firstIndex === -1 || firstIndex === lastIndex) {
        return 0;
    }

    return lastIndex - firstIndex;
}
```

## Complexity
Time: O(n), where `n` is the number of books in the array. We iterate through the list exactly once.
Space: O(1), as we only store two integer pointers regardless of the input size.