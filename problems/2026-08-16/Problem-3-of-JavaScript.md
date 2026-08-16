# The Galactic Signal Filter

## Description
You are monitoring a stream of incoming data from deep space. Due to interference, the signal is corrupted by "noise" characters. A signal is considered **valid** if it follows the pattern of being a sequence of unique characters.

Given a string `signal`, remove all duplicate characters, but you must keep the **last occurrence** of each character to preserve the integrity of the most recent data packet. Return the resulting string.

## Examples

**Example 1:**
Input: `signal = "abacaba"`
Output: `"bcaba"`
*Explanation: We keep the last 'a', the last 'b', and the last 'c'. However, since the prompt requires keeping the last occurrence of each unique character while maintaining their relative order of appearance based on their final positions, the result is "bcaba".*

**Example 2:**
Input: `signal = "leetcode"`
Output: `"letcod"`
*Explanation: 'e' appears multiple times; we only keep the 'e' at index 7. 'd' is kept. The final sequence is "letcod".*

## Solution (JavaScript)
```javascript
/**
 * @param {string} signal
 * @return {string}
 */
function filterSignal(signal) {
    const lastIndices = new Map();
    
    // Step 1: Map each character to its last occurring index
    for (let i = 0; i < signal.length; i++) {
        lastIndices.set(signal[i], i);
    }
    
    let result = "";
    const seen = new Set();
    
    // Step 2: Traverse the string and only include characters 
    // if the current index is the last index for that character.
    for (let i = 0; i < signal.length; i++) {
        if (lastIndices.get(signal[i]) === i) {
            result += signal[i];
        }
    }
    
    return result;
};
```

## Complexity
Time: O(n), where n is the length of the string, as we iterate through the signal twice.
Space: O(k), where k is the number of unique characters in the alphabet (at most O(1) if the character set is fixed, e.g., ASCII).