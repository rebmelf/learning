# Two Pointers Pattern
## When to use
- We have a **sorted array or list**
- Searching for **pairs, boundaries** or doing **in-place updates**
## Logic
- set one pointer at the **start** and one at the **end**
- move them **toward each other** based on conditions


# Sliding Window Pattern
## When to use
- working with **subarrays, substrings, sequences**
- track a **fixed-size or variable-size window** in linear time
## Logic
- Expand window by moving the **right pointer**
- Shrink from the **left** when the condition breaks
- Track **max/min/length/sum** if needed

## Tasks
- Two pointers — Find if any pair sums to target (sorted array)
- Remove duplicates in-place from a sorted list
- Sliding window — Find max sum of any subarray of size `k`
- Longest substring with at most `k` distinct characters

# Binary Search
## Definition

A search algorithm that repeatedly splits a **sorted** array (or range) in half until the target is found.
- time complexity O(log n)
- Data must be **sorted**
- Works by maintaining a **low** and **high** index

Anything can be binary searched that is "monotonic" (as one thing increases, the condition transitions from True to False or False to True)

## Binary Search on Answer Space (Search on Result)

The problem pattern is the following: 
- Find the **minimum** or **maximum value** that satisfies the condition

## Task

- Implement basic binary search on sorted array  
- Find the **first occurrence** of a target (lower bound)  
- Given an array of weights, find the **minimum capacity** to ship in D days  
- Find the square root of a number (binary search over real values)
