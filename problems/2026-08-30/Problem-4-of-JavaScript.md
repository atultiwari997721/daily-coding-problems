# The Galactic Library Cataloger

## Description
You are tasked with organizing an alien library's database. The library uses a unique system where books are stored in a sequence, but due to a gravity malfunction, some items have been replaced by "corrupted data" placeholders represented by the character `'X'`.

A sequence is considered "perfectly balanced" if the sum of all numerical values in the sequence is exactly `0`. You are given a string `s` representing the sequence, where each character is either a digit (`'1'`-`'9'`) or `'X'`. 

Your goal is to replace **every** `'X'` in the string with a single digit from `1` to `9` such that the total sum of the digits in the sequence is exactly `0`. Since all digits are positive, if it is impossible to achieve a sum of 0, return `false`.

*(Self-correction: Given the constraints of digits 1-9, a sum of 0 is impossible if there are any digits. Let's adjust the rule: The sum must equal a target value `K` provided as an argument.)*

**Revised Goal:** Given a string `s` of digits and `'X'`s, and an integer `K`, replace every `'X'` with a digit between `1` and `9` such that the sum of all characters equals `K`. Return the modified string. If it is impossible, return `null`.

## Examples

**Example 1:**
Input: `s = "1X3", K = 10`
Output: `"163"`
Explanation: 1 + 6 + 3 = 10.

**Example 2:**
Input: `s = "XX", K = 5`
Output: `"14"` (or any combination like `"23"`, `"32"`, etc.)

**Example 3:**
Input: `s = "99", K = 10`
Output: `null`
Explanation: The sum is 18, which is already greater than 10.

## Solution (JavaScript)

```javascript
/**
 * @param {string} s
 * @param {number} K
 * @return {string|null}
 */
function solveLibraryCatalog(s, K) {
    let currentSum = 0;
    let xCount = 0;
    const chars = s.split('');

    for (let char of chars) {
        if (char === 'X') {
            xCount++;
        } else {
            currentSum += parseInt(char);
        }
    }

    let remaining = K - currentSum;

    // If we don't have enough room for X's (min 1 per X) 
    // or too much room (max 9 per X)
    if (remaining < xCount || remaining > xCount * 9) {
        return null;
    }

    for (let i = 0; i < chars.length; i++) {
        if (chars[i] === 'X') {
            // Distribute the remaining value greedily
            // We need to leave at least 1 for each remaining X
            let val = Math.min(9, remaining - (xCount - 1));
            chars[i] = val.toString();
            remaining -= val;
            xCount--;
        }
    }

    return chars.join('');
}
```

## Complexity
Time: O(N), where N is the length of the string `s`. We iterate through the string twice.
Space: O(N) to store the character array for the result string.