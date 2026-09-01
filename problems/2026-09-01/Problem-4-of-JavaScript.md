# The Galactic Sensor Array

## Description
You are monitoring a 1D line of sensors in a space station. Each sensor provides a reading as an integer. Due to a calibration error, some sensors are "ghosting," meaning they report a value of `0`. 

A sensor reading is considered **stable** if it is non-zero. A sequence of sensors is **fully operational** if it contains no zeros. Given an array of sensor readings, return the length of the **longest fully operational contiguous segment**.

## Examples

**Example 1:**
Input: `sensors = [1, 5, 0, 8, 9, 3, 0, 2]`
Output: `3`
*Explanation: The segments are [1, 5], [8, 9, 3], and [2]. The longest is [8, 9, 3] with a length of 3.*

**Example 2:**
Input: `sensors = [0, 0, 0]`
Output: `0`
*Explanation: No segments contain non-zero readings.*

**Example 3:**
Input: `sensors = [7, 10, 1, 2]`
Output: `4`
*Explanation: The entire array is fully operational.*

## Solution (JavaScript)

```javascript
/**
 * @param {number[]} sensors
 * @return {number}
 */
function longestOperationalSegment(sensors) {
    let maxLen = 0;
    let currentLen = 0;

    for (let i = 0; i < sensors.length; i++) {
        if (sensors[i] !== 0) {
            // Increment the counter if the sensor is operational
            currentLen++;
            // Update maxLen if the current sequence is the longest found so far
            if (currentLen > maxLen) {
                maxLen = currentLen;
            }
        } else {
            // Reset the counter when a 0 (ghost sensor) is encountered
            currentLen = 0;
        }
    }

    return maxLen;
}
```

## Complexity
Time: **O(n)**, where *n* is the number of sensors. We traverse the array exactly once.
Space: **O(1)**, as we only use two integer variables (`maxLen` and `currentLen`) regardless of the input size.