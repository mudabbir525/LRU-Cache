# LRU Cache: Comprehensive Documentation

## 1. What is Caching? Need?

Caching is a technique of storing frequently accessed data in a temporary storage location (cache) to speed up subsequent retrievals. When an application needs to access data, it first checks if the data exists in the cache. If found (cache hit), it retrieves the data from the cache, which is faster than retrieving from the original source. If not found (cache miss), it retrieves the data from the original source and typically stores it in the cache for future use.

**Need for Caching:**
- **Performance Improvement**: Reduces data access latency by storing frequently accessed data in faster memory.
- **Bandwidth Savings**: Reduces the need to transfer data over networks, saving bandwidth.
- **Reduced Load**: Decreases the load on backend systems like databases or APIs.
- **Improved User Experience**: Provides faster response times for users.
- **Cost Efficiency**: Can reduce infrastructure costs by minimizing resource usage.

## 2. In-memory Cache: Redis and Memcache Introduction

### Redis
Redis (Remote Dictionary Server) is an open-source, in-memory data structure store that can be used as a database, cache, and message broker.

**Key Features:**
- **Versatile Data Structures**: Supports strings, hashes, lists, sets, sorted sets, bitmaps, hyperloglogs, and geospatial indexes.
- **Persistence**: Can periodically save data to disk for durability.
- **Replication**: Supports master-slave replication for high availability.
- **Transactions**: Supports atomic operations.
- **Pub/Sub Messaging**: Implements the Publish/Subscribe messaging paradigm.
- **Lua Scripting**: Allows executing scripts on the server side.
- **Automatic Eviction**: Can automatically evict keys when memory limits are reached.

### Memcached
Memcached is a free and open-source, high-performance, distributed memory object caching system designed to speed up dynamic web applications by alleviating database load.

**Key Features:**
- **Simple Key-Value Store**: Stores data as key-value pairs.
- **Distributed Architecture**: Can be scaled horizontally across multiple servers.
- **No Persistence**: Data is stored in memory only, with no disk persistence.
- **Multi-threaded**: Can utilize multiple CPU cores efficiently.
- **Simple Protocol**: Uses a simple text-based protocol.
- **Simple Data Types**: Only stores strings (serialized objects).

### Comparison: Redis vs. Memcached

| Feature | Redis | Memcached |
|---------|-------|-----------|
| Data Types | Multiple (strings, lists, sets, etc.) | Only strings |
| Persistence | Optional disk persistence | No persistence |
| Replication | Master-slave replication | No built-in replication |
| Memory Efficiency | Less memory efficient | More memory efficient |
| Multi-threading | Single-threaded (with some exceptions) | Multi-threaded |
| Transactions | Supported | Not supported |
| Eviction Policies | Multiple (LRU, LFU, etc.) | LRU only |

## 3. Cache Memory in Computer Organization

Cache memory in computer organization is a small, fast memory component that sits between the CPU and main memory (RAM) to reduce the average time to access memory. It stores copies of frequently used data from main memory for quicker access by the CPU.

### Hierarchy of Cache Memory:
1. **L1 Cache**: 
   - Smallest and fastest cache
   - Usually 32-64 KB
   - Integrated into the CPU
   - Often split into instruction cache and data cache

2. **L2 Cache**:
   - Larger and slightly slower than L1
   - Typically 256 KB to 8 MB
   - May be on the CPU chip or nearby
   - Shared among cores in multi-core processors

3. **L3 Cache**:
   - Largest and slowest of the CPU caches
   - Usually 8-32 MB
   - Shared among all cores in the processor

### Cache Memory Organization:
- **Cache Line**: Basic unit of cache storage (typically 64 bytes)
- **Cache Mapping**: Techniques to map main memory addresses to cache locations
  - **Direct Mapping**: Each memory block maps to exactly one cache line
  - **Fully Associative**: A memory block can map to any cache line
  - **Set Associative**: Compromise between direct and fully associative mapping

### Cache Performance Metrics:
- **Hit Rate**: Percentage of memory accesses found in the cache
- **Miss Rate**: Percentage of memory accesses not found in the cache
- **Hit Time**: Time to access data in the cache
- **Miss Penalty**: Additional time required to fetch data from main memory

## 4. Different Cache Replacement Strategies

When a cache is full and a new item needs to be stored, a cache replacement policy decides which items to evict. Common strategies include:

### 1. Least Recently Used (LRU)
- Discards the least recently used items first
- Based on the principle that data accessed recently is likely to be accessed again
- Implementation typically uses a linked list and a hash map
- **Pros**: Good performance for temporal locality patterns
- **Cons**: Requires tracking access history; complex implementation

### 2. First-In, First-Out (FIFO)
- Evicts the oldest item first, regardless of usage
- Implementation uses a queue
- **Pros**: Simple to implement
- **Cons**: Doesn't consider frequency or recency of access

### 3. Least Frequently Used (LFU)
- Discards items with the lowest access frequency
- Keeps count of how many times each item is accessed
- **Pros**: Works well when popularity doesn't change quickly
- **Cons**: Doesn't consider recency; items with initially high access might stay in cache too long

### 4. Most Recently Used (MRU)
- Discards the most recently used items first
- Opposite of LRU
- **Pros**: Useful in certain access patterns (e.g., scanning large files)
- **Cons**: Not suitable for most general-purpose caching

### 5. Random Replacement (RR)
- Randomly selects a cache entry for eviction
- **Pros**: Simple implementation; no overhead
- **Cons**: Performance can be unpredictable

### 6. Second Chance (Clock Algorithm)
- Modified version of FIFO
- Gives items "second chance" before eviction
- Uses a reference bit to track recent access
- **Pros**: Better than FIFO; simpler than LRU
- **Cons**: Not as effective as LRU

### 7. Adaptive Replacement Cache (ARC)
- Balances between recency and frequency
- Maintains two LRU lists, one for recent access, one for frequent access
- **Pros**: Adapts to changing access patterns; performs well in various scenarios
- **Cons**: Complex implementation

## 5. Designing LRU Cache using Doubly Linked List and Hash Map

An LRU Cache can be efficiently implemented using a combination of a doubly linked list and a hash map:

### Components:
1. **Doubly Linked List**: Tracks the order of usage, with most recently used items at the head and least recently used at the tail.
2. **Hash Map**: Provides O(1) lookup of cache entries by mapping keys to nodes in the linked list.

### Operations:
1. **Get(key)**: 
   - Look up the key in the hash map
   - If found, move the corresponding node to the front of the linked list and return the value
   - If not found, return "not found"

2. **Put(key, value)**:
   - If key exists in hash map, update the value and move the node to the front
   - If key doesn't exist:
     - If cache is full, remove the node at the tail (LRU item)
     - Create a new node with the key-value pair
     - Add the node to the front of the list
     - Add the key and node pointer to the hash map

### Complexity:
- Time Complexity: O(1) for both get and put operations
- Space Complexity: O(capacity) where capacity is the maximum number of items the cache can hold

### Advantages:
- Constant time operations
- Efficient memory usage
- Simple implementation

## 6. UML Diagram for the Design

```
+-------------------+       +----------------------+
|    LRUCache       |       |      DLLNode         |
+-------------------+       +----------------------+
| - capacity: int   |       | - key: Object        |
| - size: int       |       | - value: Object      |
| - map: HashMap    |<>---->| - prev: DLLNode      |
| - head: DLLNode   |       | - next: DLLNode      |
| - tail: DLLNode   |       +----------------------+
+-------------------+
| + LRUCache(int)   |
| + get(Object): O  |
| + put(Object, O)  |
| - addNode(DLLNode)|
| - removeNode(DLL) |
| - moveToHead(DLL) |
| - popTail(): DLL  |
+-------------------+
```

## 7. Actual Code Implementation with Explanation

```javascript
class DLLNode {
  constructor(key, value) {
    this.key = key;
    this.value = value;
    this.prev = null;
    this.next = null;
  }
}

class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.size = 0;
    this.cache = {}; // Hash map
    
    // Dummy head and tail nodes
    this.head = new DLLNode(0, 0);
    this.tail = new DLLNode(0, 0);
    
    // Connect head and tail
    this.head.next = this.tail;
    this.tail.prev = this.head;
  }
  
  // Add a node right after the head
  addNode(node) {
    node.prev = this.head;
    node.next = this.head.next;
    
    this.head.next.prev = node;
    this.head.next = node;
  }
  
  // Remove a node from the linked list
  removeNode(node) {
    const prev = node.prev;
    const next = node.next;
    
    prev.next = next;
    next.prev = prev;
  }
  
  // Move a node to the head (recently used)
  moveToHead(node) {
    this.removeNode(node);
    this.addNode(node);
  }
  
  // Remove the node before tail (least recently used)
  popTail() {
    const node = this.tail.prev;
    this.removeNode(node);
    return node;
  }
  
  // Get value by key
  get(key) {
    const node = this.cache[key];
    if (!node) return -1;
    
    // Move to head (recently used)
    this.moveToHead(node);
    return node.value;
  }
  
  // Put key-value pair
  put(key, value) {
    const node = this.cache[key];
    
    if (!node) {
      // Create new node
      const newNode = new DLLNode(key, value);
      this.cache[key] = newNode;
      this.addNode(newNode);
      this.size++;
      
      // Check if over capacity
      if (this.size > this.capacity) {
        // Remove least recently used
        const tail = this.popTail();
        delete this.cache[tail.key];
        this.size--;
      }
    } else {
      // Update value
      node.value = value;
      this.moveToHead(node);
    }
  }
}

// Example usage:
const cache = new LRUCache(2);
cache.put(1, 1); // cache is {1=1}
cache.put(2, 2); // cache is {1=1, 2=2}
console.log(cache.get(1)); // returns 1
cache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
console.log(cache.get(2)); // returns -1 (not found)
cache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
console.log(cache.get(1)); // returns -1 (not found)
console.log(cache.get(3)); // returns 3
console.log(cache.get(4)); // returns 4
```

### Explanation:

1. **DLLNode Class**:
   - Represents a node in the doubly linked list
   - Contains key, value, and pointers to previous and next nodes

2. **LRUCache Class**:
   - Uses a hash map (cache) for O(1) key lookups
   - Maintains a doubly linked list with dummy head and tail nodes
   - The most recently used items are near the head
   - The least recently used items are near the tail

3. **Helper Methods**:
   - `addNode`: Adds a node right after the head (most recently used)
   - `removeNode`: Removes a node from the linked list
   - `moveToHead`: Marks a node as recently used by moving it to the head
   - `popTail`: Removes and returns the least recently used node

4. **Main Operations**:
   - `get(key)`: Retrieves value if key exists and marks as recently used
   - `put(key, value)`: Updates existing key or adds new key and evicts LRU item if over capacity

5. **Time Complexity**:
   - All operations are O(1) time complexity
   - Hash map provides O(1) lookups
   - Doubly linked list allows O(1) inserts, removes, and moves
