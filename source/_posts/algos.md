---
title: algos
date: 2016-10-06 14:07:22
tags: 工具, algo
---

## Mask

### masking bits to 1
```
Y OR 1 = 1
```

### masking bits to 0
```
Y AND 0 = 0
```

### query the status of a bit
```
Y AND 1 = Y
```

### toggle bit value
```
Y XOR 1 = ~Y
as
Y XOR 11111111

typical usage to swap two variables without...
turn on/off

a = a ^ b
b = a ^ b
a = a ^ b

Actually, we can deduct(推导) a and b by math equation if we know both sum and substraction of a,b.
```

## BST

[Definition](https://en.wikipedia.org/wiki/Binary_search_tree)

The tree additionally satisfies the binary search tree property, which states that 
 + the key in each node must be greater than all keys stored in the left sub-tree, 
 + and smaller than all keys in the right sub-tree.



## Iterative algo

maintain loop invariant : State
 + S(0)
 + S(i) => S(i+1) : precond + logic => postcond (Assertion)
 + Then S(N)

## Design an algorithm
 + define problme
 + define loop invariants
 + define measure of progress
 + define step
 + define exit condition
 + maintain loop invariant
 + make progress
 + initial state
 + ending


Your job is only to
 + trust who passes you the baton
 + go around once
 + pass on the baton

### Picture from the middle
 + What would you like your data structure to look like when you are half done?
 + The loop invariant should state what work has been completed towards solving the problem and what works still needs to be done.
 + Loop invariant are pictures of current state.


Typical types of loop invariants
 + more of the input
 + more of the output
 + narrowed the search space
 + case analysis
 + work done

### Binary Search

if the key is contained in the original list, then the key is contained in the sublist(left,right)


### Binary Search Tree

left children <= any node <= right children

### Partition Strategy
 + Bucket Sort
 + Radix Sort : select one digit, separate cards into k piles. -- less significant first


### Counting Sort

Count to determine where records go. Go through the records in order putting them where they go.


## Recursive algo

### Divide and Conquer
 + Divide the problem into smaller instances to the same problem
 + Have a friend (recursively) solve them. Do not worry about it yourself.
 + Glue the answers together so as to obtain the answer to your larger instance.


#### check list for recursive programs

```
algorithm Alg(a,b,c):
# precond :  a is tuple, b an integer, c binary tree
# postcond:  x y z

begin::
	if(a,b,c) is sufficiently small instance, return (0,0,0)
	(a1,b1,c1) = a part of (a,b,c)
	(x1,y1,z1) = Alg(a1,b1,c1)
	(a2,b2,c2) = a different part of (a,b,c)
	(x2,y2,z2) = Alg(a2,b2,c2)
	(x,y,z) = combine((x1,y1,z1) and (x2,y2,z2))
	return (x,y,z)
end algo

# NO global variables...

```

### Recursion on Trees
A binary tree is 
 + the empty tree. (base case)
 + a node with a right and a left sub-tree.

### Some init state
 + Integer.MAX,
 + Integer.MIN,
 + 0
 + Empty
 + 1 for Product


#### Evaluation/Parse expression on Tree 



