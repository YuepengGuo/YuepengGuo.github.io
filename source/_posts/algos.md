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

Actually, we can deduct a and b by math equation if we know sum and substraction of a,b.
```
