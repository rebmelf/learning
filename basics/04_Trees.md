# General Trees
## Definition
A tree is a non-linear data structure made of nodes, where each node has
- a value
- a reference to children (usually left and right)

The top node is called the root
Each node can recursively be a root of a smaller tree (subtree)

### Binary tree
A tree, where each node has at most 2 children: left and right

## Terminology

| Term    | Meaning                          |
| ------- | -------------------------------- |
| Root    | Topmost node                     |
| Leaf    | Node with no children            |
| Parent  | Node that points to another      |
| Child   | Node that is pointed to          |
| Depth   | Distance from root               |
| Height  | Longest path from node to a leaf |
| Subtree | Tree rooted at any node          |
## Tree Traversals (Using Recursion)

1. In-Order (Left -> Root -> Right) : used in binary search trees to get sorted output
2. Pre-Order (Root -> Left -> Right): used to copy trees
3. Post-Order (Left -> Right -> Root): Used to delete or free trees

Example:

```
      1
     / \
    2   3
   / \
  4   5  
```

In-Order: 4-2-5-1-3
Pre-Order: 1-2-4-5-3
Post-Order: 4-5-2-3-1

## Tasks

- Implement a Node class and build a binary tree manually  
- Write a recursive in-order traversal function  
- Write a function to find the **sum of all nodes**  
- Count the number of leaf nodes  
- Find the **height** of the tree
# Binary Search Trees (BSTs)

## Definition
A binary search tree (BST) is a binary tree where every node follows this rule:
- left child < current node
- right child > current node
- This rule applies recursively to all subtrees

```
       10
      /  \
     5    15
    / \     \
   2   8     20
```

## Usage
- Search, insert, delete in O(log n) time if the tree is balanced
- Much faster than scanning through arrays or linked lists

##  Operations
### Search
- start at root
- if value, found
- if value smaller, go left
- if value bigger, go right
- repeat recursively

### Insert
- start like search
- When a null child spot is found, insert there. 

### Delete
3 cases
- Node is a leaf -> remove the node
- Node has 1 child -> replace node with it's child
- Node has 2 children -> find in-order successor, replace and delete that successor

- Insert values into a BST manually or in code
- Search for values
- Print BST in-order
- Find min & max value in a BST
- Implement delete

# Level order traversal (Breadth-First Search for Trees)

## Definition
Level-order traversal means visiting all nodes of a tree **level by level**, from **left to right**.

It is also called Breadth-First Traversal of a binary Tree

## Algorithm

Unlike in-order, pre-order, and post-order (which use **recursion** and a **stack**), level-order uses a **queue**.

- Put the root node in a queue.
- While the queue is not empty:
    - Remove the front node (dequeue)
    - Visit it (print or store its value)
    - Add its left child to the queue (if any)
    - Add its right child to the queue (if any)

## Tasks
- Implement level-order traversal  
- Print nodes level by level (1 per line)  
- Return a list of nodes at each depth (e.g., `[[1], [2,3], [4,5]]`)  
- Find the **maximum width** of the tree (most nodes in one level)
