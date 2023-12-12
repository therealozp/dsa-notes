
# layout
needs to have an input field to enter linked lists
- [x] space to add / remove nodes? ✅ 2023-12-12
- [x] general canvas
# pathfinding algorithms

1. create a grid of items (separate div elements)
2. make a Node element (each separate items). each items should have the following props: 

```javascript
node.x
node.y
node.isFinish => target.x == node.x && target.y == node.y
node.visited => true false
node.highlighted
// for shortest path
node.isWall => cannot be accessed

```

# data structures

### linked lists (singly and doubly)
components: 

- [ ] individual nodes / pointers to next
- [ ] edges between nodes

### graphs
components:

- [x] node (represented by value) ✅ 2023-12-12
- [x] edges (unweighted) ✅ 2023-12-12
- [ ] edges (weighted)
- [x] add node/edge ✅ 2023-12-12
- [x] adjacency list text editing field ✅ 2023-12-12