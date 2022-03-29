# Data Structures and Algos

## Basic Data Structures

#### Arrays

Arrays are indexable, Lists are not. 

#### Lists

#### Hash Tables

Maps keys to values for a highly efficient lookup. The simplest implementation has a hash function and an underlying array. The hash function maps a key to an integer, and stores the object at that index in the array.

To account for hash collisions a better implementation is to make the array smaller and calculate `hash(key) % array_size`. Then store the object in a data structure at that location, like a linked list or a binary tree.

#### ArrayList

Dynamically resizing array. Doubles in size everytime it fills. 

#### StringBuffer

For string combinations. Avoids creating and returning strings at the intermediate steps. 

#### Linked Lists

Basic list structure. Each node holds a reference to the next node. Doubly linked list also hold a reference to the one behind. Requires iterative search. Not contiguous in memory.

#### Stacks

Basically a linked list, but is LIFO-only structure. 

#### Queues

Basically a linked list, but is FIFO-only structure.

#### Priority Queues

Like a queue but every element has a certain priority. Only supports data that is inherently comparable. 

#### Trees

##### Binary (Search) Tree

Binary Tree is where every node has at most two children. BST is where for any node, the left subtree has smaller elements and the right subtree has larger elements.

#### Heaps

Tree based DS that satisfies this property: if A is a parent node of B then A is ordered with respect to B for all nodes A,B in the heap

