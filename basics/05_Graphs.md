# Definition

A **graph** is a collection of **nodes (vertices)** connected by **edges**.

# Graph Types

| Feature      | Options                   |
| ------------ | ------------------------- |
| Direction    | Directed or Undirected    |
| Weights      | Weighted or Unweighted    |
| Cycles       | Cyclic or Acyclic         |
| Connectivity | Connected or Disconnected |
# Graph Representation

**Adjacency List**
```
graph = {
    'A': ['B', 'C'],
    'B': ['D'],
    'C': [],
    'D': ['A']
}

```

**Adjacency Matrix**
```
   A B C D
A [0 1 1 0]
B [0 0 0 1]
C [0 0 0 0]
D [1 0 0 0]
```
# Graph Traversal

The tree traversal logic can be used, but we need to handle cycles and visited nodes too

## Breadth-First Search (BFS)
- Use a **queue**
- Explore all neighbors at the current level before going deeper
- Good for shortest path (in unweighted graphs)

## Depth-First Search (DFS)
- Use recursion or a stack
- Deep dive before backtracking
- Good for checking connectivity, pathfinding, cycles


# Tasks
- Represent a small directed graph using adjacency list
- Traverse it using DFS
- Traverse it using BFS
- Detect if a graph has a cycle