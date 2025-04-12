# Time & Space Complexity
## What Are These?
- Way to describe how long an algorithm takes (time complexity) and how much memory does it uses (space complexity)
- Why it matters? scalability, performance, helps in debugging or optimization

## Big-O Notation
- O(1): Constant time (e.g.: accessing an array element)
- O(n): Linear time (e.g.: looping through a list)
- O(n²): Quadratic time (e.g.: nested loops)
- O(n³): Cubic Time (e.g.: 3 nested for loops)
- O(log n): Logarithmic (log₂, but it does not matter, because we are not using constants in big O) (e.g.: binary search)
- O(n log n): Efficient sorts (e.q. Merge short)
- O(2^n): Exponential Time (e.g. fibonacci with recursion)
- O(n!): Factorial time (e.g.: generate all permutations of a list)
Visualization:
https://medium.com/better-programming/visualizing-big-o-notation-in-12-minutes-e221fc4cd2e3

## Combining complexities
### Sequential Operations
- When having two loops after each other, they don't multiply, **they add** (why?)
- O(n) + O(n) = O(n)

### Nested loops
- When one loop is inside another, their complexities **multiply**
- O(n) * O(n) = O(n²)

### Nested loops with different inputs
- O(n) + O(m) = O(n * m)

What Time Complexities Are Acceptable?

Let's say, that we need to finish an algorithm in 1 second. Our machine can do 10⁸ (100 million) operations / sec

| input size n | Acceptable complexity      | Non Acceptable Complexity    |
| ------------ | -------------------------- | ---------------------------- |
| 10           | even O(n!), O(2ⁿ)          | none                         |
| 100          | O(n²), O(n log n), O(n)    | O(2ⁿ), O(n!)                 |
| 1000         | O(n log n), O(n), O(log n) | O(n²), O(2ⁿ), O(n!)          |
| 10000        | O(n log n), O(n)           | O(n²) and worse              |
| 100000       | O(n), O(log n)             | O(n log n) only if optimized |
| 1,000,000+   | O(n), O(log n), O(1)       | O(n log n) starts to be slow |
| 10⁸          | O(n), O(log n), O(1)       | Almost anything else         |
## Space complexity
- tells us how much memory an algorith needs relatice to input size
- O(1) - the memory does not grow with input size (e.g. we are storing the sum of the elements in **one variable**)
- O(n)... - the memory grows with the input size

## Real-World Access Costs
- Even if something is O(1), not all constants are equal. Accessing data from different places has vastly different speeds
- latency numbers [Jeff Dean (Google)](https://gist.github.com/jboner/2841832)
- L1, L2, L3 Caches: super-fast memory layers between the CPU and RAM, used to speed up access to frequently-used data
	- why do we need them? 
		- RAM is fast, but not fast enough for modern CPUs -> frequently stored data are stored in cache


| Cache | Location                        | Size         | Speed | Purpose                              |
| ----- | ------------------------------- | ------------ | ----- | ------------------------------------ |
| L1    | Inside each CPU core            | ~ 32 KB      | +++   | Stores most recently accessed values |
| L2    | Still in core (slightly shared) | ~256 KB-1 MB | ++    | Backup for L1                        |
| L3    | Shared between all cores        | ~4-64 MB     | +     | Backup for L2, larger but slower     |

## Tasks

### Task 1
```
def print_elements(arr):
    for el in arr:
        print(el)

```
- How many operations would this run for n = 10, n = 10000, n = 100000
- What is the complexity?
### Task 2 
- Let's see, the computer can run 10⁸ (100 000 000) steps per second
- How big n can we handle in 2 seconds with different type of Big (O)-s

### Task 3
```
def run(n):
    count = 0
    for i in range(n):
        for j in range(n):
            count += 1
```

```
def double_elements(arr):
    result = []
    for x in arr:
        result.append(x * 2)
    return result
```
- What is the time complexity?
- What is the space complexity?
- What happens if n = 10, 1000, 1 000 000 ?
- What happens in the memory for n = 10, 1000, 1 000 000 ?
- What happens with space complexity if we build it with recursion? -> Every recursive call ads to the call stack -> the stack uses memory -> depth of recursion affects the space usage
### Task 4
What is the complexity?

| Task                                      | Complexity |
| ----------------------------------------- | ---------- |
| for i in range(n): ...                    | O(n)       |
| for i in range(n): for j in range(n): ... | O(n²)      |
| while n > 1: n = n // 2                   | O(log n)   |
| print("hi")                               | O(1)       |
### Task 5

We are reading a single value, but it's stored in:
- CPU cache
- RAM
- SSD
- Network


| Storage Type     | Time (roughly)   | Analogy (Human Time)      |
| ---------------- | ---------------- | ------------------------- |
| L1 Cache         | 0.5 nanoseconds  | Blinking an eye           |
| RAM              | 100 nanoseconds  | Walking across the room   |
| SSD              | 100 microseconds | Leaving the building      |
| HDD              | 10 milliseconds  | Driving across town       |
| Network (remote) | 150 milliseconds | Flying to another country |
- What happens if we have a list of 10 ids, and:
- Get the corresponding elements from the
	- cache
	- RAM
	- SSD
	- HDD
	- via API?
- What happens if we have a list of 1 000 000 elements?