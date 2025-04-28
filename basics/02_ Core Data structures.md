# Arrays / Lists
## Definition
- A **linear**, **ordered** collection of elements (what is linear? what is ordered?)
- Elements can be accessed by their **index**
- Arrays are mostly fixed-size
- Lists are dynamic

## Memory Storage
- elements are stored in configous memory blocks -> elements are placed right next to each other n RAM
- formula: base_address + (i* element_size)
eg.: array of integeres (int 4 byte each), base address is 100

| Index  | 0   | 1   | 2   | 3   | 4   |
| ------ | --- | --- | --- | --- | --- |
| Values | 5   | 10  | 15  | 20  | 25  |
| Memory | 100 | 104 | 108 | 112 | 116 |
## Complexities

| Operation                                    | Complexity       | Why?                                                                                                                                        |
| -------------------------------------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| Access                                       | O(1)             | Elements are stored in **contiguous memory**; index math directly gives the address.                                                        |
| Append to end                                | O(1) (amortized) | Most of the time, appending goes to the next free slot. Occasionally, resizing happens, which is O(n), but spread out over many operations. |
| Insert at Index                              | O(n)             | All elements after the index must be **shifted one position to the right**.                                                                 |
| Remove from end                              | O(1)             | No shifting required; just update length and forget the last item.                                                                          |
| Remove from front or middle                  | O(n)             | All elements after the removed item must be **shifted left** to fill the gap.                                                               |
| Loop through all elements                    | O(n)             | You visit each element once — one operation per item.                                                                                       |
| Search for value (unsorted)                  | O(n)             | You may have to check every element to find the target.                                                                                     |
| Search for value (sorted with binary search) | O(log n)         | You can halve the search space each time — like guessing a number with “higher/lower.”                                                      |
## List resizing
- Python list, Java ArrayList are Dynamic arrays, which means they use **resizable arrays**
### Resizing
- the append() or add() an element, but no room left
- Then the system:
	- allocates a new, larger array (usually 2x the current size)
	- copies all the old elements to the new array
	- adds the new element
- The old array is discarded

### Why is appending still O(1) on average
- resizing happens rarely and the cost is spread out over many appends
- this is amortized O(1)

## Pros and cons
Pro:
- fast access
- ordered
- simple to use
- cache friendly (continouos-memory)
Cons:
- Fixed -size (arrays)
- Costly insert/delete
- Resize copying costly (for dinamyc arrays)

## Tasks:

1. Loop through all elements, print each item with its index
2. Sum all numbers in an array
3. Reverse an array without using built-in reverse methods
4. Find the maximum element
5. Move zeros to the end without sorting

For each solution provide the time/space complexity
# Strings
## Definition
A sequence of characters. Strings are immutable

What will be the result of these codes
```
text = "hello"
text[0] = "H"

# error
```

```
text = "hello"
text.charAt(0) = 'H'

// error
```

```
text = "hello"
text = "H" + text[1:]

This works
```

## Common operations

| Operation             | Time Complexity | Notes                                                                                                     |
| --------------------- | --------------- | --------------------------------------------------------------------------------------------------------- |
| Access chat at index  | O(1)            | Just like arrays                                                                                          |
| Loop through string   | O(n)            | Each character is visited once                                                                            |
| Concatenation (+)     | O(n)            | may create a new string (some language dependent magic means the may, eg. Java might use a Stringbuilder) |
| Slicing / substring   | O(k)            | k = slice lenght                                                                                          |
| String equality check | O(n)            | Compares character by character                                                                           |
| Search for substring  | O(n * m) worst  | n times checking the m long substring                                                                     |
## Tasks

1. Reverse a string ("hello" -> "olleh")
2. Check for palindrome ("racecar" -> true)
3. Count characters (frequency map), ("banana" -> {'b:' 1, 'a': 3, 'n': 2})
4. First non-repeating character ("swiss" -> "w" )
5. Group anagrams (["eat", "tea", "tan", "ate", "nat", "bat"] -> [["eat", "tea", "ate"], ["tan", "nat"], ["bat"] ]

 For each solution provide the time/space complexity
# Sets

## Definition
An unordered collection of unique elements, which contains no duplicates

## Common Operations

| Operation        | Time Complexity | Notes                                |
| ---------------- | --------------- | ------------------------------------ |
| Add element      | O(1) (average)  | Hashing determines where to store it |
| Remove element   | O(1) (average)  | Hash-based lookup                    |
| Check membership | O(1) (average)  | USe the hash of the value            |
| Iterate over set | O(n)            | All elements must be visited         |

## How a set works

It uses a hash table

### What is a hash table?
A data structure that stores key-value pairs, and allows for very fast lookups, insertions and deletions, tipically in O(1) time on average

### How it works?
- We provide a key (this is the value we are storing)
- The hash table uses a hash function to convert the key to a number
- That is used to decide where to store the value in memory (a bucket)
- When we are trying to find a vale, the same hash is used to jump to the right spot

### Important
- Collisions can happen (two keys hash to the same spot)
	- This can be handled by chaining or open addressing
	- keys must be hashable (fixed, comparable, and consistent)

Let's get back to Sets!
### Inserting into a Set

- We are adding a value to the set
- The set calculates `hash(value)`
- It checkts the corresponding buket in it's internal table
- If empty -> inserts
- If not empty, collison

Checking membership
- Calculate `hash("value")`
- Jump straight to the right bucket
- check iff "value" is there

Since Sets are relying on hash tables, the value needs to be hashable
- python: strings, numbers, tuples (if elements are immutable)
- in Java: anything that properly implements `.hashCode()` and `.equals()`


## Tasks:

1. Check for duplicates [1,2,3,2] ->  True
2. Remove duplicates [1,2,3,2] ->  [1,2,3]
3. Common elements in two lists ([1,2,3],[2,3,4]-> [2,3])
4. Longest substring without repeating characters ("abcabcbb) -> 3)

 For each solution provide the time/space complexity

# Hash Maps / dictionaries

## Definition
- A collection of key/value pairs
- Each key maps to a specific value
- Very fast lookups, tipically O(1) average time
- Keys must be unique


## Time Complexities

| Operation    | Time Complexity | Notes                                        |
| ------------ | --------------- | -------------------------------------------- |
| Insert (put) | O(1) average    | Hash key is used, quick find of proper place |
| Get (lookup) | O(1) average    | Jumps directly to hash                       |
| Delete       | O(1) average    | Locate by hash, remove                       |
| Iterate      | O(n)            | Visit every key-value pair                   |
## How hashing works
1. Hashing the key
	- On the key, call the hash function, it creates the hash code (a large number)
2. Map the hash to to a bucket index
	- The hash code is reduced (e.g. hash % number_of_buckets /modulus/, gives the remainder)
	- The remainder shows which bucket should be addressed
3. Access the bucket
	- The bucket can be accessed by it's number
	- Check the first element, if it's the key, then found
	- Else we have a **collision**, scan the bucket

## Resizing
- Hash maps have a **load factor**, which means how full the table can get before resizing (for java HashMap it's 0,75)
- When the map get's too full, then it resizes (doubles the size of buckets), rehashes all existing keys (can be expensive, performance drop can be seen when inserting a lot of elements together)

## Hash Collisions
- If two keys end up in the same bucket, the hash map stores both there (in a list or a tree)
- If there are two many collisions, we have slower lookups (closer to O(n))
- Good hash functions make collisions rare

## Ordering

- Python: after 3.7+  preserves insertion order
- Java: Hahmap no ordering quarantee, if needs to be ordered, we can use LinkedHashMap

In python the keys can only be **immutable**.
## Tasks

1. Word counter: Loop through words, count with a map
2. Find the most frequent numbers
3. Find the target sum of two numbers from a list of numbers

