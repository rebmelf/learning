# Stacks and Queues

## Definitions

### Stacks

A **stack** is a **linear data structure** that **follows the Last In, First Out (LIFO)** principle.  
You can **only access or remove the most recently added element**.

- linear: elements are arranged one after another
- has two main operations:
	- push: add an element to the top
	- pop: remove the top element
- One optional operation:
	- peek or top: view the top element without removing it

#### Common stack operations

| Operation    | Time Complexity | Meaning                       |
| ------------ | --------------- | ----------------------------- |
| Push (add)   | O(1)            | Add item to the top           |
| Pop (remove) | O(1)            | Remove top item               |
| Peek (view)  | O(1)            | View the top without removing |

### Queue

A **queue** is a **linear data structure** that **follows the First In, First Out (FIFO)** principle.  
You can **only access or remove the least recently added element**.

| Operation                   | Time Complexity | Meaning                         |
| --------------------------- | --------------- | ------------------------------- |
| Enqueue (add to back)       | O(1)            | Add item at the end             |
| Dequeue (remove from front) | O(1)            | Remove from the front           |
| Peek (view front)           | O(1)            | View the first without removing |

## Common Real-World Applications

| Stacks                                   | Queues                           |
| ---------------------------------------- | -------------------------------- |
| Undo/redo in text editors                | Task scheduling                  |
| Browser back button                      | Print jobs in printer queue      |
| Recursive function call management       | Breadth-first search in graphs   |
| Syntax parsing (e.g., matching brackets) | Multi-threaded messaging systems |

## Variants

| Variant                      | Idea                                                              | Where to use                                           |
| ---------------------------- | ----------------------------------------------------------------- | ------------------------------------------------------ |
| Dequeue (Double-ended Queue) | You can add/remove at both from and back                          | sliding window problems, cache implementations         |
| Priority Queue               | Items are ordered by priority, not by arrival time                | scheduling tasks with urgency levels                   |
| Monotonic Stack / Queue      | A special structure that keeps elements sorted as they are pushed | Used in advanced problems (eg.: Next Greater Element*) |

\* For each element in an array, **find the next element to its right that is greater than itself**.  If no such element exists, return **-1** for that position.
## Memory
- Stacks and queues are lightweight, because they only keep references to elements
- this means that if we forget to dequeue or pop items, that **can leak memory** (useless objects kept alive)

## Call Stack in recursion
- Every function call (especially recursion) uses an internal stack -> deep recursion can cause **stack overflow**, which means the call stack runs out of memory
## Questions:

- Why is a stack good for undo but not for scheduling?
- Why is it important that stack and queue operations are O(1)?
## Tasks

1. Implement a basic stack
2. Implement a basic queue
3. Reverse a list using a stack
4. Check for balanced parentheses eg.: / (((()))) -> True , ()) -> false
5. Implement a queue using 2 stacks

# Recursion

**Recursion** is when a function **calls itself** to solve a smaller piece of the problem, until it reaches a **base case**.
Every recursive function must have:
- **Base case**: when to stop the recursion
- **Recursive case**: the function calls itself on a smaller input

## How recursion uses a Call stack
Every time a function calls itself:
- A **new frame** is pushed onto the **call stack**
- When base is reached:
	- The function **returns** and the **frames pop off** the stack one by one
- That's why too much recursion can cause **stack overflow error**!
## Common mistakes

| Mistake                | What happens                         |
| ---------------------- | ------------------------------------ |
| No base case           | Infinite recursion -> stack overflow |
| Wrong base case        | Wrong answer, even if it stops       |
| Forget to shrink input | Infinite recursion                   |
## Some more
### Every Recursive Function Can be Written Iteratively
- Anything can be solved with loops
- recursion often makes code shorter and easier to understand

### Memory usage
- Every function call uses memory (new stack frame)
- Deep recursion can create millions of calls, it will be very slow
- Some programming language limit recursion depth

### Tail recursion
- some languages can optimize tail recursion
- tail recursion: the recursive call is the very last thing a function does
- Some compilers can reuse the current stack frame, no stack overflow

### When to use?
- **Self-Similarity**: "Can I solve this by solving a smaller version of the same problem?"
- **Divide and Conquer**: "Can I split the problem in two halves (like in Merge Sort)?"
- **Backtracking**: Trying all possible paths (used in Sudoku solvers, permutations)

## Tasks
1. Print numbers from 1 to n
2. Calculate factorial
3. Sum of elements in a list
4. Fibonacci number
5. Reverse a string recursively
6. Check if a string is a palindrome recursively
7. Count the number of digits in a number
8. Find the maximum in a list recursively
9. Sum of digits of a number
