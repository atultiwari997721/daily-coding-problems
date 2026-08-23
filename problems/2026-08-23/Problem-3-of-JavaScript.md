# The Social Distancing Seating Plan

## Description
You are tasked with organizing a seating chart for a long, narrow workshop. The room has `n` seats arranged in a single row, represented by an array of integers `seats`, where `0` represents an empty seat and `1` represents an occupied seat.

Due to social distancing guidelines, no two occupied seats can be adjacent. You are given a number `k`, representing the number of **new** attendees who must be seated. 

Return `true` if it is possible to seat all `k` new attendees such that no two occupied seats are adjacent, and `false` otherwise. Note that the original occupants are already seated according to the rules, but you must ensure your new placements do not violate the adjacency rule with existing occupants or other new occupants.

## Examples

**Example 1:**
Input: `seats = [1, 0, 0, 0, 1]`, `k = 1`
Output: `true`
Explanation: You can place an attendee at index 2. The array becomes `[1, 0, 1, 0, 1]`.

**Example 2:**
Input: `seats = [1, 0, 0, 0, 1]`, `k = 2`
Output: `false`
Explanation: Placing an attendee at index 2 makes index 1 and 3 unusable. You cannot place a second person.

**Example 3:**
Input: `seats = [0, 0, 0, 0, 0]`, `k = 3`
Output: `true`
Explanation: You can place attendees at indices 0, 2, and 4.

## Solution (JavaScript)

```javascript
/**
 * @param {number[]} seats
 * @param {number} k
 * @return {boolean}
 */
var canPlaceAttendees = function(seats, k) {
    let count = 0;
    
    for (let i = 0; i < seats.length; i++) {
        // Check if the current seat is empty
        if (seats[i] === 0) {
            // Check if left and right neighbors are empty (or out of bounds)
            let prevEmpty = (i === 0 || seats[i - 1] === 0);
            let nextEmpty = (i === seats.length - 1 || seats[i + 1] === 0);
            
            if (prevEmpty && nextEmpty) {
                // Place the attendee
                seats[i] = 1;
                count++;
                
                // Optimization: If we've met the goal, return true early
                if (count === k) return true;
            }
        }
    }
    
    return count >= k;
};
```

## Complexity
Time: O(n), where `n` is the length of the `seats` array, as we iterate through the list at most once.
Space: O(1), as we modify the array in place and use a constant amount of extra space.