#linked-lists #design

when inserting a key, not currently in the cache, while the cache is at capacity, we have to evict the **least-recently-used** key, hence the name. 

to resolve the issue with $O(1)$ insertions and $O(1)$ retrievals, we use a **linked-list** to store the order of access, and each entry in the cache is a linked-list node, which can be used directly to update its order.

```python
class LLNode:
    def __init__(self, key=None, value=None):
        self.val = value
        self.key = key
        self.prev = None
        self.next = None

class LL:
    def __init__(self):
        self.head = LLNode()
        self.tail = LLNode()
        self.head.next = self.tail
        self.tail.prev = self.head

    def insertAtHead(self, node):
        # assuming node is its own standalone thing
        # unlink both sides
        if node.prev:
            node.prev.next = node.next
        if node.next:
            node.next.prev = node.prev

        # push node to start aka head
        newest = self.head.next
        newest.prev = node
        self.head.next = node

        node.next = newest
        node.prev = self.head

    def deleteFromTail(self):
        nodeToDelete = self.tail.prev
        # unlink
        self.tail.prev = nodeToDelete.prev
        # returns the key to evict / delete
        return nodeToDelete.key

class LRUCache:

    def __init__(self, capacity: int):
        self.size = 0
        self.capacity = capacity
        self.cache = {}
        self.container = LL()

    def get(self, key: int) -> int:
        if key in self.cache:
            node = self.cache[key]
            self.container.insertAtHead(node)
            return node.val
        else:
            return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            node = self.cache[key]
            node.val = value
            self.container.insertAtHead(node)
        else:
            # print(self.cache, self.size)
            if self.size >= self.capacity:
                # evict LRU
                keyToDelete = self.container.deleteFromTail()
                self.size -= 1
                del self.cache[keyToDelete]
            newNode = LLNode(key, value)
            self.container.insertAtHead(newNode)
            self.cache[key] = newNode
            self.size += 1
```