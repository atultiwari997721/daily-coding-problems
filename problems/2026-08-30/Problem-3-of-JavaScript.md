# The Galactic Signal Echo

## Description
You are analyzing signals from deep space. Due to interference, every signal is transmitted as a string of characters, but it is "echoed" exactly once immediately after its first appearance. 

However, a cosmic glitch causes the **very last character** of the transmission to be potentially truncated if the signal length is odd. 

Given a string `s`, determine if the string is a valid "echoed" transmission. A string is valid if it can be partitioned into two identical halves, potentially with the last character of the second half missing.

In other words:
1. If the length of the string is even, the first half must be identical to the second half.
2. If the length of the string is odd, the first half must be identical to the second half (excluding the last character of the original echo).

## Examples

**Example 1:**
Input: `s = "abcabc"`
Output: `true`
Explanation: "abc" == "abc".

**Example 2:**
Input: `s = "abcab"`
Output: `true`
Explanation: The first half is "ab" (indices 0,1). The second half starts at index 2. "abcab" split is "ab" and "cab". Wait, the logic is: the first half of the total length is "ab". The remaining part is "cab". Since "ab" matches the prefix of "cab", it returns true.

**Example 3:**
Input: `s = "hello"`
Output: `false`
Explanation: Half length is 2. First part "he", second part "llo". "he" does not match "ll".

## Solution (JavaScript)
```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var isValidEcho = function(s) {
    if (s.length <= 1) return false;
    
    // Calculate the midpoint. 
    // For even length 6, mid is 3. First half [0,2], Second [3,5]
    // For odd length 5, mid is 2. First half [0,1], Second [2,4]
    const mid = Math.floor(s.length / 2);
    
    const firstHalf = s.slice(0, mid);
    const secondHalf = s.slice(mid);
    
    // Check if the first half matches the beginning of the second half
    // In the odd case, we only need to match up to the length of the firstHalf
    return secondHalf.startsWith(firstHalf);
};
```

## Complexity
Time: O(n), where n is the length of the string, due to slicing and string comparison.
Space: O(n) to store the sliced substrings.