# The Harmony Index

## Description
In a digital music library, a playlist is considered "Harmonious" if every song has a unique "vibe" score. You are given an array of integers `vibes` representing the vibe scores of songs in a playlist.

A playlist is **Stable** if we can remove **exactly one** song such that the remaining songs all have distinct vibe scores.

Given an array `vibes`, return `true` if the playlist is **Stable**, otherwise return `false`.

## Examples

**Example 1:**
Input: `vibes = [1, 2, 2, 3]`
Output: `true`
Explanation: Removing one `2` leaves `[1, 2, 3]`, which are all distinct.

**Example 2:**
Input: `vibes = [1, 1, 2, 2]`
Output: `false`
Explanation: Removing any one song still leaves a duplicate pair (e.g., removing a `1` leaves `[1, 2, 2]`).

**Example 3:**
Input: `vibes = [5, 5, 5, 1]`
Output: `false`
Explanation: Removing one `5` leaves `[5, 5, 1]`, which still has duplicates.

## Solution (JavaScript)
```javascript
/**
 * @param {number[]} vibes
 * @return {boolean}
 */
function isStable(vibes) {
    const counts = new Map();
    
    // Count frequencies of each vibe score
    for (const vibe of vibes) {
        counts.set(vibe, (counts.get(vibe) || 0) + 1);
    }
    
    // Find how many numbers appear more than once
    let duplicateCount = 0;
    let problematicElement = null;
    
    for (const [vibe, count] of counts.entries()) {
        if (count > 1) {
            duplicateCount++;
            problematicElement = vibe;
        }
    }
    
    // If no duplicates exist, we can't remove one to make it distinct (it already is)
    // If more than one distinct number repeats, we can't fix it by removing one element
    if (duplicateCount !== 1) {
        return false;
    }
    
    // If exactly one number is repeated, it must appear exactly twice
    // (If it appears 3+ times, removing one instance leaves a duplicate)
    return counts.get(problematicElement) === 2;
}
```

## Complexity
Time: **O(n)**, where *n* is the length of the `vibes` array, as we iterate through the array once to build the frequency map and once through the unique keys.

Space: **O(k)**, where *k* is the number of unique vibe scores stored in the Map.