# Standard Allocator #

---

The standard Allocator in C++ manages a memory heap. It stores data in blocks of space indicated by the user when needed. It keeps track of busy blocks of memory.
The following functions are supported:
  * (constructor)  Construct allocator object (public member function)
  * (destructor)   Allocator destructor (public member function)
  * address	Return address (public member function)
  * allocate	Allocate block of storage (public member function)
  * deallocate	Release block of storage (public member function)
  * max\_size	Maximum size possible to allocate (public member function)
  * construct	Construct an object (public member function)
  * destroy	Destroy an object (public member function)


This project consisted of simulating the standard allocator through the use of sentinel values.

## Custom Allocator ##

---

We created an Allocator class which contained the following methods:
  * (constructor)
  * allocate
  * deallocate
  * destroy

The main functionality of the allocator is in the constructor, allocate and deallocate methods.

By default we have the following functions:
  * Default copy, destructor, and copy assignment
  * Allocator (const Allocator< T >&);
  * ~Allocator ();
  * Allocator& operator = (const Allocator&);

### (constructor) ###
The constructor initializes the two sentinels which indicate the amount of bytes of free space available.

Allocator<int, 100> x;

will initialize our memory like:
| 92 | ... | ... | ... | 92|
|:---|:----|:----|:----|:--|

Each sentinel value is an **int** and occupies 4 bytes. Because of this, the free space is 92 bytes.

### allocate ###
Sets aside space (when available) and creates new sentinels which indicate the amount of space that is busy. The sentinels for a busy block have a negative value.

x.allocate(1);

After allocating 1 integer our memory will look like:
| -4 |  -  | -4 | 80 |  ... | 80 |
|:---|:----|:---|:---|:-----|:---|

### deallocate ###

Frees a busy block that was previously allocated. The elements in the array will not be destructed.

x.deallocate(1);

Will return our memory to the initial state:
| 92 | ... | ... | ... | 92|
|:---|:----|:----|:----|:--|

There are two cases we had to deal with when deallocating:
  * A busy block with no adjacent free blocks
  * A busy block with 1 or 2 free adjacent blocks

When the busy block has a free block adjacent to it, both blocks are to be merged.