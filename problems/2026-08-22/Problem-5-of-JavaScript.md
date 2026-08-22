# The Galactic Grocery Sorter

## Description
You are helping an alien robot organize its pantry. The robot has a list of items, where each item is represented by a string. Because the robot's scanners are glitchy, some items are stored in a "corrupted" format: they contain a sequence of digits that represent how many times that item should be duplicated.

An item is "corrupted" if it ends with a digit. For example, `"apple2"` should be interpreted as `"apple", "apple"`. If an item does not end in a digit, it is just a single item.

Given an array of strings `pantry`, return a new array where all items are expanded according to their counts. Maintain the original order of the items.

## Examples

**Example 1:**
Input: `pantry = ["milk1", "bread2", "cheese"]`
Output: `["milk", "bread", "bread", "cheese"]`

**Example 2:**
Input: `pantry = ["alien-fruit3", "water1"]`
Output: `["alien-fruit", "alien-fruit", "alien-fruit", "water"]`

**Example 3:**
Input: `pantry = ["chips", "soda0"]`
Output: `["chips"]` (Note: a count of 0 removes the item entirely)

## Solution (JavaScript)

```javascript
/**
 * @param {string[]} pantry
 * @return {string[]}
 */
function sortPantry(pantry) {
    const result = [];

    for (const item of pantry) {
        // Use a regular expression to capture the name and the trailing digit
        const match = item.match(/^(.+?)(\d+)$/);

        if (match) {
            const name = match[1];
            const count = parseInt(match[2], 10);
            
            for (let i = 0; i < count; i++) {
                result.push(name);
            }
        } else {
            // If no trailing digit, treat as a single item
            result.push(item);
        }
    }

    return result;
}
```

## Complexity
Time: O(N * K), where N is the number of items in the array and K is the average number of times an item is duplicated (or 1 if no digit). We iterate through the array once and perform an inner loop for duplication.

Space: O(M), where M is the total number of items in the final expanded list, as we are creating a new array to store the result.