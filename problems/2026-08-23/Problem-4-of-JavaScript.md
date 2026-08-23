# The Galactic Signal Relay

## Description
You are managing a sequence of $N$ interstellar signal relays arranged in a line. Each relay $i$ has a signal strength $S_i$. 

A "Stable Transmission" is defined as a contiguous subarray $[i, j]$ such that the **Bitwise AND** of all elements in the subarray is greater than $0$.

However, there is a catch: you are allowed to perform **at most one** "Signal Boost". A Signal Boost allows you to choose exactly one index $k$ and replace $S_k$ with any integer value $X$ (where $0 \le X \le 2^{31}-1$).

Your goal is to find the **maximum possible length** of a Stable Transmission after performing at most one Signal Boost.

## Examples

### Example 1
**Input:** `S = [1, 2, 4, 8]`
**Output:** `4`
**Explanation:** If we change any element to $15$ ($1111_2$), all elements can share the bit at position 0, 1, 2, and 3. By setting one element to a value that covers the intersection of its neighbors, we can form a transmission of length 4.

### Example 2
**Input:** `S = [7, 7, 0, 7]`
**Output:** `3`
**Explanation:** Replacing the `0` at index 2 with a `7` makes the entire array `[7, 7, 7, 7]`, which is a stable transmission of length 4. Wait—if we replace index 2, the length is 4. *Correction:* The input constraints define stability as AND > 0. If we replace `0` with `7`, the AND of the whole array is `7 > 0`. Output is 4.

### Example 3
**Input:** `S = [1, 0, 1]`
**Output:** `3`
**Explanation:** Replacing the middle `0` with `1` results in `[1, 1, 1]`, which has an AND of `1 > 0`.

## Solution (JavaScript)
```javascript
/**
 * @param {number[]} S
 * @return {number}
 */
function maxStableTransmission(S) {
    const n = S.length;
    if (n === 0) return 0;
    if (n === 1) return S[0] > 0 ? 1 : 1;

    let maxLength = 0;

    // We can boost one relay to any value. 
    // To maximize a subarray [i, j] containing k,
    // we need all elements in [i, j] (excluding k) to share at least one bit.
    // That is: (S[i] & ... & S[k-1] & S[k+1] & ... & S[j]) > 0.
    
    // We check every bit position (0 to 30).
    for (let bit = 0; bit < 31; bit++) {
        let mask = 1 << bit;
        
        // Transform array into 1 if bit is set, else 0
        let binary = S.map(x => (x & mask) ? 1 : 0);
        
        // Find longest subarray with at most one '0'
        let left = 0;
        let zeroCount = 0;
        for (let right = 0; right < n; right++) {
            if (binary[right] === 0) zeroCount++;
            
            while (zeroCount > 1) {
                if (binary[left] === 0) zeroCount--;
                left++;
            }
            maxLength = Math.max(maxLength, right - left + 1);
        }
    }

    return maxLength;
}
```

## Complexity
Time: **O(31 * N)**, where $N$ is the number of relays. This simplifies to **O(N)**.
Space: **O(N)** to store the binary transformation, though this can be optimized to **O(1)** by calculating the bit checks on the fly.