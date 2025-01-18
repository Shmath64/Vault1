- Hash tables, key-value pairs
- Trees
- Binary Search Trees
- Splay Trees
- Heaps
- Graphs
	- Prim's Algorithm


#### A-lists
Unlike arrays that have a fixed size with integer index values.
A-list is a list of cons pairs. Is a dynamic structure, instead of using indexes, we look at the *car* of each element
`(defvar alist'( (0 . "Zero") (1. "One") (2. "Two") ))`

`(car '(5 6))` = `(car '(5 . 6))`
`(cons 5 6)` = `(5 . 6)`
`(cdr '(5 6))` >> `(6)` ; cons cell has 5 as car and `(6 . nil)` as the cdr
`(cdr '(5 . 6))` >> `6` ; cons cell holds 5 and 6

To search,
`(assoc 3 *words)`
To add:
`(push '(4 . "Four") *words*)`
To remove:
`(remove 3 *words* :key 'car)`
### Hash Tables
Hash tables hash keys to map them to indexes of an array
Simplest hash function: `h(k) = k mod n` where k is the key and n is length of the array

Two ways to resolve *hash collisions*:
- **Chaining**: Let the array point to a linked list 
- **Probing**: Put the value somewhere else in the table (where it technically doesn't belong)
	- **Linear**: Move it forward a fixed amount until it "fits"
		- "Clustering Issue": potential to build up lots of consecutive keys in table
	- **Quadratic**: Auxiliary hash function has $i^2$ to help distribute the keys

With linear probing/insertion, our **auxiliary hash function** may look like:
$h(k,i) = (h'(k) + i)\ mod\ n$ ; where i is the iteration number
With quadratic probing/insertion
$h(k,i) = (h'(k) + i + i^2)\ mod\ n$

When removing elements from a hash table, we need to set the value there to "deleted" so other elements may be inserted, but we can still probe to elements passed "deleted"

### Trees
- Made up of nodes,
- Each node has a value and a list of nodes below it.
- Nodes CANNOT exist in multiple branches
- "Leaf" or "Leaf nodes" have no nodes beneath it.
```Lisp
(defstruct (tree-node (:conc-name nil)) key children) ...)
```

```Lisp
;Recurisve approach
(defun search-tree (e node &optional (found nil))
	(cond ((null node) found) ; Indicates end of node or empty tree
		(found e)
		((listp node) (search tree e (cdr node) (search-tree e (car node))))
		((equal e (key node)) e)
		(t (search-tree e (children node)))))

;Using dolist
(defun search-tree (e node)
	(when node
		(if (equal e (key node)) e
			(dolist (b (children node))
				(let ((res (search-tree e b)))
					(when res (return res)))))))
```

##### General Tree Traversals
**Depth-First Search** Process a node and its children before moving back up
- Pre-order: Current, all children
- In-order: Left, Current, Right (Only for BST)
- Post-order: All Children, current

**Breadth-First Search** (AKA, **Level Order**): Vist every node on a level before going to the next level.

#### Binary Search Trees (BST)
Every node has a left child and a right child. Elements to the left are "less than" the current element and elements to the right are "greater than" the current element.
#CBH And review code around this!

### Splay Trees
Is a binary tree
Every time a node is visited, the node "splays" (propagates) to the top of the tree through a series of rotations. The tree will always be in the sorted order.

Functions:
- **tree-rotate**: (target node, its parent, and its grandparent) rotates the node up to be in the parent's position and makes proper connections with grandparent.
	- This is called repeatedly to "rotate" the node up to root.
- **node-chain**: (item, root &optional chain) creates a list of nodes to target
	- 
- **splay**: (node, chain) repeats tree-rotate until node is at root
	- 
- **st-search**: If the value exists, splay it to the top of the tree and return new root
- 