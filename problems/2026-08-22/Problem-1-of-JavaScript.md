# The Galactic Signal Booster

## Description
You are monitoring a deep-space communications array. The array receives a sequence of signal integers, where each integer represents the power level of a data packet.

A "Signal Boost" occurs whenever a data packet has a power level **strictly greater** than the power level of the packet immediately preceding it. Given an array of signal levels, write a function `countSignalBoosts` that returns the total number of times a signal boost occurs throughout the sequence.

## Examples

**Example 1:**
Input: `signals = [1, 2, 3, 4, 5]`
Output: `4`
Explanation: Every step increases (1 to 2, 2 to 3, 3 to 4, 4 to 5).

**Example 2:**
Input: `signals = [5, 4, 3, 2, 1]`
Output: `0`
Explanation: No packet is greater than the one before it.

**Example 3:**
Input: `signals = [1, 3, 2, 5, 5, 8]`
Output: `3`
Explanation: Boosts occur at: 1→3, 2→5, and 5→8. Note that 5→5 is not a boost.

## Solution (JavaScript)
```javascript
/**
 * @param {number[]} signals
 * @return {number}
 */
function countSignalBoosts(signals) {
    let boosts = 0;
    
    // Iterate from the second element to the end
    for (let i = 1; i < signals.length; i++) {
        // Compare current signal with the previous one
        if (signals[i] > signals[i - 1]) {
            boosts++;
        }
    }
    
    return boosts;
}
```

## Complexity
Time: O(n) where n is the length of the input array, as we perform a single pass through the sequence.
Space: O(1) as we only use a single counter variable regardless of input size.