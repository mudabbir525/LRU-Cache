# LRU Cache Implementation & Visualizer

This project implements a Least Recently Used (LRU) cache data structure with a web-based visualizer to demonstrate how the cache operates. The implementation uses a combination of a doubly linked list and a hash map to achieve O(1) time complexity for both get and put operations.

## 📋 Project Components

1. **Documentation**: Comprehensive write-up explaining caching concepts and LRU implementation
2. **Code Implementation**: JavaScript implementation of an LRU cache 
3. **Visualizer**: Interactive web application to visualize LRU cache operations
4. **UML Diagram**: Design diagram showing the structure of the LRU cache

## 🔍 What is an LRU Cache?

An LRU (Least Recently Used) Cache is a type of cache that removes the least recently used items first when the cache reaches its capacity. This is based on the principle that recently accessed items are likely to be accessed again in the near future.

## 🏗️ Implementation Details

The LRU Cache is implemented using:

- **Doubly Linked List**: Maintains the order of usage with most recently used items at the head and least recently used at the tail
- **Hash Map**: Provides O(1) lookup of cache entries

### Key Operations:

- `get(key)`: Retrieves the value associated with the key and marks it as recently used
- `put(key, value)`: Adds or updates a key-value pair in the cache

### Time Complexity:

- Get operation: O(1)
- Put operation: O(1)

## 🖥️ Visualizer Features

The web-based visualizer allows you to:

- Initialize a cache with a custom capacity
- Perform get and put operations
- View the doubly linked list structure in real-time
- See the hash map contents
- Follow operation logs to understand the cache behavior
- View step-by-step execution of operations

## 📚 Topics Covered in Documentation

1. What is caching and its importance
2. In-memory cache solutions: Redis and Memcached
3. Cache memory in computer organization
4. Different cache replacement strategies
5. Detailed explanation of LRU cache design
6. UML diagram for the design
7. Complete code implementation with explanations

## 🚀 How to Run the Project

1. Clone this repository
```bash
git clone https://github.com/mudabbir525/LRU-Cache.git
```

2. Open the `index.html` file in a web browser
```bash
cd LRU-cache
open index.html  # on macOS
# or
start index.html  # on Windows
```

## 🧩 Project Structure

```
├── README.md                  # This file
├── index.html                 # Main HTML file with visualizer
├── documentation.md           # Comprehensive write-up about caching and LRU
```

## 📝 Assignment Requirements Fulfilled

- [x] Explanation of caching concepts
- [x] Introduction to Redis and Memcached
- [x] Explanation of cache memory in computer organization
- [x] Discussion of different cache replacement strategies
- [x] Detailed design of LRU cache using doubly linked list and hash map
- [x] UML diagram
- [x] Complete code implementation with explanation
- [x] Visualizer for MRU-LRU sequence and map status

## 🛠️ Technologies Used

- HTML5
- CSS3
- JavaScript
- Bootstrap 5
- GitHub (for version control)

## 📊 Future Enhancements

- Add animation for visualizing node movements
- Implement additional cache replacement strategies for comparison
- Add performance metrics visualization
- Support for larger dataset visualization

## 👨‍💻 Author

Mohammed Mudabbir Pasha

