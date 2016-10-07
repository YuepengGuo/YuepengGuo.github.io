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