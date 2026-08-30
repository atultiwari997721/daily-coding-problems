# The Galactic Signal Alignment

## Description
You are receiving a sequence of signals from deep space, represented as a list of integers. Due to interference, the signal is "misaligned" if there is an **odd** number of non-zero elements present in the sequence. 

Your task is to determine if the sequence is currently aligned. If the count of non-zero elements is even, return `True`; otherwise, return `False`.

Additionally, to "clean" the signal, you must return a new list containing all the non-zero elements from the original list in their original relative order.

Return a tuple containing:
1. A boolean: `True` if the count of non-zero elements is even, `False` otherwise.
2. A list: The cleaned sequence of non-zero elements.

## Examples

**Example 1:**
Input: `signals = [0, 5, 0, 3, 0]`
Output: `(True, [5, 3])`
*Explanation: There are 2 non-zero elements (5 and 3). 2 is even, so True. The cleaned list is [5, 3].*

**Example 2:**
Input: `signals = [1, 2, 3, 0]`
Output: `(False, [1, 2, 3])`
*Explanation: There are 3 non-zero elements (1, 2, 3). 3 is odd, so False. The cleaned list is [1, 2, 3].*

## Solution (Python)
```python
def align_galactic_signal(signals):
    # Extract all non-zero elements
    cleaned = [s for s in signals if s != 0]
    
    # Check if the count of non-zero elements is even
    is_aligned = len(cleaned) % 2 == 0
    
    return (is_aligned, cleaned)
```

## Complexity
Time: O(n), where n is the length of the input list, as we iterate through the list once to filter the elements.
Space: O(n) in the worst case, where we store the cleaned list of non-zero elements.