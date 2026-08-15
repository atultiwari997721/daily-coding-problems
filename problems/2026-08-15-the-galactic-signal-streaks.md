# The Galactic Signal Streaks

## Description
You are monitoring a stream of incoming signal intensities from a distant galaxy. The signal is represented as an array of integers. A **"Stable Streak"** is defined as a contiguous subarray where every element is strictly greater than the previous element.

Given an array of integers `signals`, return the **length** of the longest "Stable Streak".

## Examples

**Example 1:**
*   **Input:** `signals = [1, 3, 5, 4, 7]`
*   **Output:** `3`
*   **Explanation:** The longest stable streak is `[1, 3, 5]`, which has a length of 3.

**Example 2:**
*   **Input:** `signals = [2, 2, 2, 2]`
*   **Output:** `1`
*   **Explanation:** Since elements must be *strictly* greater, no streak longer than 1 exists.

**Example 3:**
*   **Input:** `signals = [10, 9, 8, 7]`
*   **Output:** `1`
*   **Explanation:** The signals are strictly decreasing, so the longest streak is just a single element.

## Solution (JavaScript)
```javascript
/**
 * @param {number[]} signals
 * @return {number}
 */
function longestStableStreak(signals) {
    if (signals.length === 0) return 0;

    let maxLength = 1;
    let currentStreak = 1;

    for (let i = 1; i < signals.length; i++) {
        // Check if the current signal is strictly greater than the previous one
        if (signals[i] > signals[i - 1]) {
            currentStreak++;
        } else {
            // Reset streak if the pattern breaks
            currentStreak = 1;
        }
        
        // Update global maximum
        if (currentStreak > maxLength) {
            maxLength = currentStreak;
        }
    }

    return maxLength;
}
```

## Complexity
Time: O(n), where `n` is the length of the `signals` array, as we iterate through the array exactly once.
Space: O(1), as we only use two variables to track the current and maximum streak lengths.