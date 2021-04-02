# Data Structures Introduction

##### [reference](https://www.freecodecamp.org/news/the-top-data-structures-you-should-know-for-your-next-coding-interview-36af0831f5e3/)

## What is Data Structure

A data structure is a container that stores data in a specific layout. This "layout" allows data structure to be efficient in some operations and inefficient in others. The goal is to understand data structures so that you can pick the data structure that's most optimal for the problem at hand.

Well, why do we need Data Structures?
No matter what kind of problem you are solving, you must deal with data. Based on different scenarios, data needs to be stored in a specific format.

Here is a list of commonly used data structures:
- [Arrays](#arrays)
- [Stacks](#stacks)
- [Queues](#queues)
- [Linked Lists](#linked-list)
- [Graphs](#graphs)
- [Trees](#trees)
- [Tries](#trie) (they are effectively trees, but it's still good to call them out separately)
- [Hash Tables](#hash-table)
## Arrays

An array is the simplest and most widely used data structure. Other data structures like stacks and queues are derived from arrays.
Say there's an image of simple array of size 4, containing elements (1, 2, 3 and 4).
Each data element is assigned a positive numerical value called **index**, which corresponds to the position of that item in the array. The majority of languages define the starting index of the array as 0.

#### Two types of arrays:
- One-dimensional arrays
- Multi-dimensional arrays (*arrays within arrays*)

#### Basic Operations on Arrays:
- **Insert** - inserts an element at a given index
- **Get** - returns the element at a given index
- **Delete** - deletes an element at a given index
- **Size** - gets the total number of elements in an array

## Stacks

We are all familiar with the **Undo** option(ctrl + z), which is present in almost every application. The idea is to store the previous states of your work in the memory in such an order that the last one appears first. This can't be done just by using arrays. This is where the **Stack** comes in handy.
Say you have a pile of books placed in a vertical order. In order to get a certain book that's somewhere in the middle, you will need to remove all the books placed on top of it. This is how the **LIFO (Last In First Out)** method works.

#### Basic Operations of Stacks:
- **Push** - inserts an element at the top
- **Pop** - returns the top element after removing from the stack
- **isEmpty** - returns true if the stack is empty
- **Top** - returns the top element without removing from the stack

## Queues

Similar to Stack, Queue is another linear data structure that stores the element in a sequential manner. The only significant difference between Stack and Queue is that instead of using the **LIFO** method, Queue implmenets the **FIFO(First In FIrst Out)** method.
Say there is a line of people waiting at a ticket booth. If a new person comes, that person will join the line from the end, not from the start. The person in the front of the line will leave after purchasing the ticket.

#### Basic Operations of Queue:
- **Enqueue()** - inserts an element to the end of the queue
- **Dequeue()** - removes an element from the start of the queue
- **isEmpty()** - returns true if the queue is empty
- **Top()** - returns the first element of the queue

## Linked List

A linked list is another important linear data structure which might look similar to arrays at first, but differs in memory allocation, internal structure and how operations of insertion and deletion are carried out.
A linekd list is like a chain of nodes, where each node contains information like data and a pointer to the succeeding node in the chain. There's a head pointer, which points to the first element of the linked list, and if the list is empty then it simply points to null or nothing.
Linked lists are used to implement file systems, has tables, and adjacency lists.

#### Two types of Linked Lists:
- **Singly Linked List** (*Unidirectional*)
- **Doubly Linked List** (*Bi-directional*)
#### Basic Operations of Linked List:
- **InsertAtEnd** - inserts a given element at the end of the linked list
- **InsertAtHead** - inserts a given element at the start/head of the linked list
- **Delete** - delete a given element from the linked list
- **DeleteAtHead** - deletes the first element of the linked list
- **Search** - returns the given element from a linked list
- **isEmpty** - returns true if the linked list is empty

## Graphs

A graph is a set of nodes that are connected to each other in the form of a network. Nodes ares also called **vertices**. A **pair(x,y)** is called an **edge**, which indicates that vertex **x** is connected to vertex **y**. An edge may contain weight/cost, showing how much cost is required to traverse from vertex x to y.

#### Types of Graphs:
- **Unidirected Graph**
- **Directed Graph**

#### In programming language, graphs can be represented using two forms:
- **Adjacency Matrix**
- **Adjacency List**

#### Common graph traversing algorithms:
- **Breadth First Search**
- **Depth First Search**

## Trees

A tree is a hierarchical data structure consisting of vertices (nodes) and edges that connect them. Trees are similar to graphs, but they key point that differentiates a tree from the graph is that a cycle cannot exist in a tree.
Trees are extensively used in Artifical Intelligence and complex algorithms to provide an efficient storage mechanism for problem-solving.

#### Different types of trees:
**N-ary Tree**, **Balanced Tree**, **Binary Tree**, **Binary Serach Tree**, **AVL Tree**, **Red Black Tree**, **2-3 Tree**.
Out of the above, **binary Tree** and **Binary Serach Tree** are the most commonly used trees.

## trie

This is also known as "Prefix Trees", is a tree-like data structure which proves to be quite efficient for solving problems related to strings. It provides fast retrieval, and is mostly used for searching words in a dictionary, providing auto suggestions in a search engine, and even for IP routing.

## Hash Table

Is a process used to uniquely identify objects and store each object at some pre-calculated unique idnex called its "key". So, the object is stored in the form of a "key-value" pair, and the collection of such items is called a "dictionary". Each object can be searched using that key. There are different data structures based on hashing, but the most commonly used data structure is the **hash table**.
Hash tables are generally implemented using arrays.
The performance of hashing data structure depends upon three factors:
- **Hash Function**
- **Size of the Hash Table**
- **Collision Handling Method**

