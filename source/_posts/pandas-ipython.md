---
title: pandas ipython
date: 2016-08-20 15:45:39
tags: 工具
---

### CH3 pandas data structures

#### NumPy ndarrays
creation
```
import numpy as np
ar1 =  np.array([0,1,2,3,4])
ar1.shape
ar1.ndim
ar3 =  np.arange(12)
#start,end(exclusive),step
ar4 =  np.arange(3,10,3)
#linspace, start,end and number of elements
ar5 = np.linspace(1,4,4)
ar6 = np.ones((2,3,4))
ar7 = np.zeros((4,2))
# identity matrix
ar8 = np.eye(3)
# diagonal matrix
ar9 = np.diag((2,1,4,6))
# rand
np.random.seed(100)
ar10 = np.random.rand(3)
#empty
ar11 = np.empty((3,2))
#tile
np.tile(np.array([[1,2],[6,7]]),(3,4))

sar = np.array(['google','alibaba','yahoo','facebook'])
sar.dtype

ar = np.arange(5)
ar[::-1]

```
<!-- more -->
the default dtype in NumPy is float, in the case of string, dtype is the length of the longest string in the array.


Indexing and slicing

Arrays can be reversed using the ::-1 idiom as 

multi-dimensional arrays are indexed using tuples of integers as ar[1,1]

arrays can be sliced by using the following syntax: ar[startIndex,endIndex,stepValue]
ar[1:5:2]
obtain the first n-elements using ar[:n], the implicit assumption here is taht startIndex=0, and Step=1
start an element 4 until the end ar[4:]
slice array with step 3 as ar[::3]

Array masking
evenMask=(ar%2 == 0)

evenNums = ar[evenMask]

replace value with masking

ar[ar==' '] = 'USA'


Copys vs views
ar[:3].copy()

# dot operator
ar.dot(ar2)

# transpose
ar.T
np.transpose(ar)

#array equal
np.array_equal(ar,ar2) 
#or
np.all(ar==ar2)

#reduce
ar=np.arange(1,5)
ar.prod()
np.prod(ar,axis=0)
np.prod(ar,axis=1)
ar.sum()
ar.mean()
np.median(ar)
#statistics
ar.mean()
ar.std()
ar.var(axis=0)
ar.cumsum()
#logical
np.all(ar<11)
np.any((ar%7)==0)

broadcasting

shape manipulation

#flatten by
ar.ravel()
ar.T.ravel()
ar.reshape(3,5)

#array sort
ar.sort(axis=1)


Data Structures in pandas
series : 1D NumPy with an array of lables.
dataframe : 2D labeled array. a dataframe column is a series strcture. dictionary of both columns and rows are indexed.
panel :  3D array (items->axis 0, major_axis->axis 1, minor_axis->axis 2)

```
USData = pd.DataFrame(np.array([[249.62,8900],[282.16,12680],[309.35,14940]]),
                      columns=['Population(M)','GDP($B)'],
                      index=[1990,2000,2010]
                      )


ChinaData = pd.DataFrame(np.array([[1133.68,390.28],[1266.83,1198.48],[1339.72,6988.47]]),
                      columns=['Population(M)','GDP($B)'],
                      index=[1990,2000,2010]
                      )

US_China_Data = {'US':USData,'China':ChinaData}

p = pd.Panel(US_China_Data)

```

