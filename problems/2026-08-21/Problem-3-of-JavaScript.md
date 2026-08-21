# The Galactic Signal Decorator

## Description
You are receiving a signal from space represented as an array of integers. However, due to cosmic interference, some signals are "muffled." A muffled signal is defined as an integer that appears exactly once in the entire array.

Your task is to identify and return the **sum** of all muffled signals. If there are no muffled signals, return 0.

## Examples

**Example 1:**
Input: `signal = [1, 2, 3, 2]`
Output: `4`
Explanation: 1 and 3 are unique (muffled). 1 + 3 = 4.

**Example 2:**
Input: `signal = [5, 5, 5, 5]`
Output: `0`
Explanation: There are no unique numbers.

**Example 3:**
Input: `signal = [1, 1, 2, 3, 3, 4]`
Output: `6`
Explanation: 2 and 4 are unique. 2 + 4 = 6.

## Solution (JavaScript)

```javascript
/**
 * @param {number[]} signal
 * @return {number}
 */
function sumUniqueSignals(signal) {
    const counts = new Map();
    let totalSum = 0;

    // Count occurrences of each signal
    for (const num of signal) {
        counts.set(num, (counts.get(num) || 0) + 1);
    }

    // Add up only those that appear exactly once
    for (const [num, count] of counts) {
        if (count === 1) {
            totalSum += num;
        }
    }

    return totalSum;
}
```

## Complexity
Time: **O(n)**, where `n` is the length of the signal array, as we iterate through the array once and the map once.
Space: **O(n)**, in the worst case where all elements are unique and stored in the Map.