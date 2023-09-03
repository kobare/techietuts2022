---
layout: post
title:  "Creating Custom JavaScript Data Structures and Algorithms: A Practical Tutorial"
author: Denis Kobare
date:   2024-02-04 11:00:00 +0300
img: /assets/img/svg/javascript.svg
categories: programming
sub_category: javascript
type: concepts
technology: javascript
permalink: "category/:categories/javascript/concepts/:year:month/:title"
---


### Introduction

JavaScript is a versatile and powerful language used primarily for web 
development, but its capabilities extend far beyond that. One of the fundamental 
aspects of programming is understanding data structures and algorithms. In this 
section, we'll explore the implementation of custom data structures and 
algorithms in JavaScript, complete with code explanations, use cases, and 
performance considerations.



<br>
### The Power of Custom Data Structures

Custom data structures allow you to tailor your code to specific use cases, 
improving efficiency and readability. We'll start by creating three essential 
custom data structures: linked lists, trees, and graphs.



<br>
### Linked Lists

A linked list is a linear data structure where elements, called nodes, are 
connected by pointers. This structure allows for dynamic size and efficient 
insertion/deletion operations. Let's implement a basic singly linked list:

{% highlight javascript %}

class Node {
    constructor(data) {
        this.data = data;
        this.next = null;
    }
}

class LinkedList {
    constructor() {
        this.head = null;
        this.length = 0;
    }

    append(data) {
        const newNode = new Node(data);
        if (!this.head) {
            this.head = newNode;
        } else {
            let current = this.head;
            while (current.next) {
                current = current.next;
            }
            current.next = newNode;
        }
        this.length++;
    }

    // Other methods: insert, delete, search, etc.
}

{% endhighlight %}


Use case: Linked lists are great for scenarios where you frequently insert or 
remove elements, such as implementing undo/redo functionality in applications.



<br>
### Trees

Trees are hierarchical data structures with a root element and a set of child 
elements, each of which can have its own children. We'll implement a binary 
search tree (BST), a commonly used type of tree:

{% highlight javascript %}

class TreeNode {
    constructor(value) {
        this.value = value;
        this.left = null;
        this.right = null;
    }
}

class BinarySearchTree {
    constructor() {
        this.root = null;
    }

    insert(value) {
        const newNode = new TreeNode(value);
        if (!this.root) {
            this.root = newNode;
        } else {
            this._insertRecursive(this.root, newNode);
        }
    }

    _insertRecursive(node, newNode) {
        if (newNode.value < node.value) {
            if (!node.left) {
                node.left = newNode;
            } else {
                this._insertRecursive(node.left, newNode);
            }
        } else {
            if (!node.right) {
                node.right = newNode;
            } else {
                this._insertRecursive(node.right, newNode);
            }
        }
    }

    // Other methods: search, delete, traverse, etc.
}

{% endhighlight %}


Use case: Binary search trees are useful for efficient searching and sorting 
operations, such as finding the closest elements in a range or implementing 
autocomplete functionality.



<br>
### Graphs

Graphs are versatile data structures that consist of nodes connected by edges. 
They can represent a wide range of relationships. We'll create a simple directed 
graph:

{% highlight javascript %}

class Graph {
    constructor() {
        this.nodes = new Map();
    }

    addNode(node) {
        this.nodes.set(node, []);
    }

    addEdge(node1, node2) {
        if (this.nodes.has(node1) && this.nodes.has(node2)) {
            this.nodes.get(node1).push(node2);
        }
    }

    // Other methods: removeNode, removeEdge, traverse, etc.
}

{% endhighlight %}


Use case: Graphs can model various scenarios, such as social networks, maps, or 
dependency management systems.



<br>
### Custom Algorithms in JavaScript

Now that we have a solid foundation in custom data structures, let's explore 
some essential algorithms. We'll focus on sorting and searching algorithms.



<br>
### Sorting Algorithms

Sorting is a fundamental operation in computer science. Here's a simple 
implementation of the bubble sort algorithm:

{% highlight javascript %}

function bubbleSort(arr) {
    const n = arr.length;
    for (let i = 0; i < n - 1; i++) {
        for (let j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap arr[j] and arr[j+1]
                [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
            }
        }
    }
}

{% endhighlight %}


Use case: Bubble sort is straightforward to understand, but it's not the most 
efficient sorting algorithm. It's suitable for small arrays or when simplicity 
is more important than performance.



<br>
### Searching Algorithms

Searching algorithms help find specific elements within a data structure. Let's 
implement a binary search, which is a fast searching algorithm for sorted arrays:

{% highlight javascript %}

function binarySearch(arr, target) {
    let left = 0;
    let right = arr.length - 1;
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        if (arr[mid] === target) {
            return mid; // Found the target!
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1; // Target not found
}

{% endhighlight %}


Use case: Binary search is highly efficient for finding elements in sorted 
arrays. It's commonly used in scenarios where you need to quickly locate items, 
such as in search engines or dictionaries.



<br>
### Performance Considerations

While custom data structures and algorithms offer flexibility, it's essential to 
consider their performance characteristics. For example:

- Time Complexity: Different operations (e.g., insertion, deletion, searching) 
may have different time complexities for each data structure or algorithm. 
Understand these complexities to choose the right one for your use case.

- Space Complexity: Some data structures may consume more memory than others. 
Consider the memory requirements of your application.

- Balancing: For tree-based structures, balancing can be crucial to maintaining 
optimal performance. Unbalanced trees may lead to skewed performance in certain 
operations.



<br>
### Conclusion

In summary, custom data structures and algorithms empower you to solve complex 
problems efficiently. By understanding their implementation, use cases, and 
performance considerations, you'll be better equipped to choose the right tools 
for your programming needs. Start experimenting with these concepts in your 
JavaScript projects, and you'll unlock a new level of programming versatility. 
Happy coding!


<br>
*Thanks for reading, see you in the next one!*
