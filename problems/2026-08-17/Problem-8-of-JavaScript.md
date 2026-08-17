# The Balanced Library Shelf

## Description
You are organizing a library shelf. You are given a string `shelf` consisting of characters representing books. Each book has a specific "weight" represented by its character code.

A shelf is considered **"perfectly balanced"** if the sum of the character codes of all books on the left half of the shelf is exactly equal to the sum of the character codes of all books on the right half.

Given a string `shelf` with an **even** length, determine if it is perfectly balanced. If the length is odd, it can never be perfectly balanced, so return `false`.

## Examples

**Example 1:**
Input: `shelf = "ad"`
(ASCII: 'a' = 97, 'd' = 100)
Output: `false`

**Example 2:**
Input: `shelf = "afde"`
(Left: 'a' + 'f' = 97 + 102 = 199. Right: 'd' + 'e' = 100 + 101 = 201)
Output: `false`

**Example 3:**
Input: `shelf = "book"`
(Left: 'b' + 'o' = 98 + 111 = 209. Right: 'o' + 'k' = 111 + 107 = 218)
Wait, let's look for a balanced one:
Input: `shelf = "abcy"`
(Left: 97 + 98 = 195. Right: 99 + 121 = 220)
Input: `shelf = "adbc"`
(Left: 97 + 100 = 197. Right: 98 + 99 = 197)
Output: `true`

## Solution (JavaScript)

```javascript
/**
 * @param {string} shelf
 * @return {boolean}
 */
function isPerfectlyBalanced(shelf) {
    const n = shelf.length;
    
    // An odd length shelf cannot be split into two equal halves
    if (n % 2 !== 0) return false;
    
    let leftSum = 0;
    let rightSum = 0;
    const mid = n / 2;
    
    for (let i = 0; i < mid; i++) {
        leftSum += shelf.charCodeAt(i);
    }
    
    for (let i = mid; i < n; i++) {
        rightSum += shelf.charCodeAt(i);
    }
    
    return leftSum === rightSum;
}
```

## Complexity
Time: O(n), where n is the length of the string, as we iterate through the shelf once to calculate the sums.
Space: O(1), as we only use a few integer variables regardless of the input size.