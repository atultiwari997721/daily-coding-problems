# The Galactic Sensor Array

## Description

You are monitoring a 1D line of sensors in space represented by an array of integers `sensors`, where each integer represents the signal strength detected at that position.

Due to a calibration error, some sensors are "dead" (represented by `0`). A sensor is considered "functional" if it is **greater than 0**. You want to find the **Shortest Functional Gap**: the length of the smallest contiguous subarray that contains **exactly** two functional sensors. If there are fewer than two functional sensors in the entire array, return `0`.

## Examples

**Example 1:**
*   **Input:** `sensors = [0, 5, 0, 0, 3, 0]`
*   **Output:** `3`
*   **Explanation:** The functional sensors are at index 1 and 4. The subarray `[5, 0, 0, 3]` contains them. The distance between them (including the sensors themselves) is 4, but the problem asks for the shortest gap of *exactly* two functional sensors. Wait, let's clarify: indices are 1 and 4. The subarray `[5, 0, 0, 3]` has length 4.

**Example 2:**
*   **Input:** `sensors = [2, 0, 0, 1, 4, 0]`
*   **Output:** `2`
*   **Explanation:** The functional sensors are at 0, 3, and 4. The pairs are (0, 3) distance 4, (3, 4) distance 2. The smallest is 2.

## Solution (Python)

```python
def shortest_functional_gap(sensors: list[int]) -> int:
    # Get the indices of all functional sensors
    functional_indices = [i for i, val in enumerate(sensors) if val > 0]
    
    # If there are fewer than two functional sensors, return 0
    if len(functional_indices) < 2:
        return 0
    
    min_gap = float('inf')
    
    # Calculate the distance between every adjacent pair of functional sensors
    # The distance between index i and j (inclusive) is (j - i + 1)
    for i in range(len(functional_indices) - 1):
        gap = functional_indices[i+1] - functional_indices[i] + 1
        if gap < min_gap:
            min_gap = gap
            
    return int(min_gap)
```

## Complexity
Time: **O(N)**, where N is the length of the `sensors` array, as we iterate through the list once to find indices and once more through the reduced list of indices.

Space: **O(N)** in the worst case (where all sensors are functional), to store the indices of the functional sensors.