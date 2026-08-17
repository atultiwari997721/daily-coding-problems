# The Binary Palindrome Checker

## Description
A "Binary Palindrome" is a number whose binary representation (without leading zeros) reads the same forwards and backwards.

Given a non-negative integer `n`, return `true` if its binary representation is a palindrome, and `false` otherwise.

*Note: For `n = 0`, the binary representation is `"0"`, which is a palindrome.*

## Examples

**Example 1:**
* **Input:** `n = 9`
* **Output:** `true`
* **Explanation:** `9` in binary is `1001`. `1001` reversed is `1001`.

**Example 2:**
* **Input:** `n = 10`
* **Output:** `false`
* **Explanation:** `10` in binary is `1010`. `1010` reversed is `0101`, which is not equal to `1010`.

**Example 3:**
* **Input:** `n = 7`
* **Output:** `true`
* **Explanation:** `7` in binary is `111`. `111` reversed is `111`.

## Solution (JavaScript)
```javascript
/**
 * @param {number} n
 * @return {boolean}
 */
var isBinaryPalindrome = function(n) {
    // Convert the number to its binary string representation
    const binaryStr = n.toString(2);
    
    // Use two pointers to check if the string is a palindrome
    let left = 0;
    let right = binaryStr.length - 1;
    
    while (left < right) {
        if (binaryStr[left] !== binaryStr[right]) {
            return false;
        }
        left++;
        right--;
    }
    
    return true;
};
```

## Complexity
* **Time:** O(log n), because the number of bits in the binary representation of `n` is proportional to `log2(n)`.
* **Space:** O(log n), as we store the binary string representation of the number.