# The Chrono-Sync Buffer

## Description
You are designing a synchronization buffer for a distributed system that receives data packets with unique timestamps. Due to network latency, packets may arrive out of order. 

You are given a list of `packets`, where each `packet` is a list `[timestamp, data]`. A packet is considered "ready to process" if all packets with a smaller timestamp have already been processed. 

The system processes packets in a specific way:
1. It maintains a **buffer**.
2. When a packet arrives, you add it to the buffer.
3. After every arrival, you must check if the "next expected packet" (starting from timestamp `0`, then `1`, then `2`, etc.) is in the buffer.
4. If it is, you "process" it and look for the next one.
5. Return the sequence of data in the order they were processed.

## Examples

**Example 1:**
*   **Input:** `packets = [[2, "B"], [0, "A"], [1, "C"]]`
*   **Output:** `["A", "C", "B"]`
*   **Explanation:**
    1. Packet [2, "B"] arrives. Buffer: {2: "B"}. Next expected: 0. (Nothing to process)
    2. Packet [0, "A"] arrives. Buffer: {2: "B", 0: "A"}. Next expected: 0. Process "A". Next expected: 1. (1 is not in buffer).
    3. Packet [1, "C"] arrives. Buffer: {2: "B", 1: "C"}. Next expected: 1. Process "C". Then check for 2. Process "B".

**Example 2:**
*   **Input:** `packets = [[1, "X"], [3, "Y"], [0, "Z"], [2, "W"]]`
*   **Output:** `["Z", "X", "W", "Y"]`

## Solution (Python)

```python
def process_packets(packets):
    # Use a dictionary to store packets that arrived out of order
    buffer = {}
    result = []
    
    # This tracks the timestamp we are currently waiting for
    next_expected = 0
    
    for timestamp, data in packets:
        # Add incoming packet to the buffer
        buffer[timestamp] = data
        
        # Continuously check if the next expected packet is available
        while next_expected in buffer:
            result.append(buffer[next_expected])
            # Clean up the buffer to keep space complexity optimal
            del buffer[next_expected]
            next_expected += 1
            
    return result
```

## Complexity
*   **Time: O(N)**: Each packet is added to the dictionary once and removed from the dictionary once. The `while` loop runs a total of N times across the entire execution of the function.
*   **Space: O(N)**: In the worst-case scenario (e.g., the packets arrive in reverse order: `[N-1, N-2, ... 0]`), we store all N packets in the dictionary before we can start processing them.