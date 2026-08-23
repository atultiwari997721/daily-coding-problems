# The Quantum Data Stream Synchronizer

## Description

You are building a high-frequency trading system that receives data packets from multiple quantum nodes. Due to network jitter, packets arrive out of order and with varying latencies. Each packet consists of an `id` (integer) and a `value` (integer).

A packet with `id = i` can only be processed if all packets with `id < i` have already been processed. 

However, there is a **Quantum Buffer**:
1. You have a buffer of size `K`.
2. You can hold packets in the buffer if their `id` is greater than the current expected `id`.
3. If a packet arrives with an `id` exactly equal to the `expectedId`, it is processed immediately, and any subsequent packets in the buffer that can now be processed are also handled.
4. If a packet arrives with an `id > expectedId + K`, it is considered "corrupted" and must be dropped.
5. If the buffer is full and you receive a packet with `id > expectedId`, you must drop the packet with the *largest* `id` currently in the buffer to make room for the new one.

Given an array of packets `[id, value]` and the buffer capacity `K`, return the sum of all `value`s of the packets that were successfully processed in order.

## Examples

**Example 1:**
*   **Input:** `packets = [[1, 10], [3, 30], [2, 20]]`, `K = 1`
*   **Output:** `60`
*   **Explanation:** Process 1, then 2, then 3. All processed.

**Example 2:**
*   **Input:** `packets = [[1, 10], [4, 40], [2, 20]]`, `K = 1`
*   **Output:** `30`
*   **Explanation:** 1 is processed. 4 arrives; expected 2, so 4 is buffered. 2 arrives; 2 is processed. Now 3 is expected, but 4 is in buffer. 4 is processed. Sum = 10 + 20 + 40 = 70? Wait, if K=1, packet 4 is held. When 2 arrives, we process 2. Then 3 is missing, so we stop.

## Solution (JavaScript)

```javascript
/**
 * @param {number[][]} packets
 * @param {number} K
 * @return {number}
 */
function synchronizePackets(packets, K) {
    let expectedId = 1;
    let totalValue = 0;
    // Map to store buffered packets: id -> value
    const buffer = new Map();
    // Max-priority storage for buffer keys to handle "drop largest"
    let bufferIds = [];

    for (const [id, value] of packets) {
        if (id === expectedId) {
            totalValue += value;
            expectedId++;
            
            // Check if buffered packets can now be processed
            while (buffer.has(expectedId)) {
                totalValue += buffer.get(expectedId);
                buffer.delete(expectedId);
                bufferIds = bufferIds.filter(bid => bid !== expectedId);
                expectedId++;
            }
        } else if (id > expectedId && id <= expectedId + K) {
            if (buffer.size >= K) {
                // Drop largest ID in buffer
                bufferIds.sort((a, b) => b - a);
                const largestId = bufferIds.shift();
                buffer.delete(largestId);
            }
            buffer.set(id, value);
            bufferIds.push(id);
        }
        // If id < expectedId or id > expectedId + K, packet is ignored/dropped
    }

    return totalValue;
}
```

## Complexity
Time: **O(N * K log K)**, where N is the number of packets. In each step, we potentially sort the buffer IDs (size K). This could be optimized to **O(N log K)** using a Max-Heap.
Space: **O(K)** to store the buffered packets.