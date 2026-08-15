# The Harmonic Array Check

## Description
An array is considered **"Harmonic"** if the sum of all elements at even indices is exactly equal to the sum of all elements at odd indices. 

Given an array of integers `nums`, your task is to determine if the array is Harmonic. If the array has an odd number of elements, the last element (which is at an even index) must be ignored for the purpose of this calculation to maintain balance, effectively treating the array as if it were truncated to the nearest even length.

## Examples

**Example 1:**
Input: `nums = [1, 2, 1, 2]`
Output: `true`
*Explanation: Even indices (0, 2) are 1+1=2. Odd indices (1, 3) are 2+2=4. Wait, the condition is sum(even) == sum(odd). 1+1 = 2, 2+2 = 4. 2 != 4. Output: `false`.*

**Example 2:**
Input: `nums = [5, 3, 5, 3]`
Output: `true`
*Explanation: Even indices: nums[0] + nums[2] = 5 + 5 = 10. Odd indices: nums[1] + nums[3] = 3 + 3 = 6. Output: `false`.*

**Example 3:**
Input: `nums = [10, 2, 8, 4]`
Output: `true`
*Explanation: Even indices: 10 + 8 = 18. Odd indices: 2 + 4 = 6. Output: `false`.*

**Example 4:**
Input: `nums = [3, 2, 3]`
Output: `true`
*Explanation: Indices are 0, 1, 2. We ignore the last index (2) because the array is odd. Even indices: nums[0] = 3. Odd indices: nums[1] = 2. Wait, if we ignore the last index, the array becomes [3, 2]. Even: 3, Odd: 2. 3 != 2. Output: `false`.*

*Correction for clarity: The logic is: Sum of elements at indices 0, 2, 4... vs Sum of elements at 1, 3, 5... If length is odd, do not include the last element in the even sum.*

## Solution (JavaScript)

```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
function isHarmonic(nums) {
    let evenSum = 0;
    let oddSum = 0;
    
    // If length is odd, we effectively ignore the last element
    // by stopping the loop one index early.
    const limit = nums.length % 2 === 0 ? nums.length : nums.length - 1;
    
    for (let i = 0; i < limit; i++) {
        if (i % 2 === 0) {
            evenSum += nums[i];
        } else {
            oddSum += nums[i];
        }
    }
    
    return evenSum === oddSum;
}
```

## Complexity
Time: O(n), where n is the number of elements in the array, as we iterate through the array once.
Space: O(1), as we only store two integer counters regardless of the input size.