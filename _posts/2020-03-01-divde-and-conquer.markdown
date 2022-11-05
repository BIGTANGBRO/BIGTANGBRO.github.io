---
layout: post
title:  Divde and Conquer Algorithms
date:   2020-02-09 15:05:55 +0700
image:  /assets/images/blog/post-3.jpg
author: Jiaxuan Tang
tags:   Python, Algorithms
---
**Notes from course Data Structure and algorithms from coursera**

> Divide and conquer algorithm is very important and always referred as '分治法'.

##### 这周的算法思想是divide and conquer(分治法)。分治法是把问题分成最小的规模，逐个解决，再合并。二分搜索，归并排序与快速排序都是比较经典的例子。

>下面会把每个基本算法代码写出来，但事实上他们还是能被优化的。


<br>

#### 二分搜索 Binary Search

**Binary search**
```python
def binarySearch(A,low,high,key):
    A.sort()
    if high<low:
        return low - 1
    mid = int(low + (high-low)/2)
    if key == A[mid]:
        return mid
    elif key < A[mid]:
        return binarySearch(A,low,mid-1,key)
    else:
        return binarySearch(A,mid+1,high,key)

```
<br>

#### 普通的搜索算法 linear search

**linear Search**
```python
##linear search

def linearSearch(array1,element):
    found = False
    for i in array1:
        if i == element:
            found = i
            break
    return found

#recursive way

def linearSearchRecursion(A,low,high,key):
    #low = lower bound
    #high = higher bound

    if high<low:
        return "Not Found"
    if A[low] == key:
        return low
    return linearSearchRecursion(A,low+1,high,key)
```


PS: ***Master theorem*** 主定理：
T(n)=aT([n/b])+O(n^d)
其时间复杂度分为三种情况
- 1.O(n^d) if d>logba; 
- 2.T(n)=O(n^d log(n)) if d == logba 
- 3.T(n) =O(n^(logba)) if d < logba

<br>

### 排序中经常会用到divide and conquer的思想。

- 选择排序
- 归并排序
- 计数排序
- 快速排序

<br>

#### 选择排序 Selection sorting

> 选择排序是嘴简单的排序算法，类似的还有插入排序与冒泡排序，时间复杂度为O(n^2)

**选择排序代码的两种方法**

```python
##Sorting problem
#selection sorting 

def selectionSorting(a):
    for i in range(len(a)):
        minIndex = i
        for j in range(i+1,len(a)):
            if a[j] < a[minIndex]:
                minIndex = j
        a[i],a[minIndex] = a[minIndex],a[i]
    return a
        
def selectionSorting2(a):
    for i in range(len(a)):
        k = min(a[i:len(a)])
        indexMin = i+a[i:].index(k)
        a[i],a[indexMin] = a[indexMin],a[i]
    return a
```

<br>

#### 归并排序 Merge Sort

> 归并排序是一种效率较高的算法，思想是把一个list分为极小的部分，各个排序之后merge还原成sorted list. 时间复杂的为O(nlog(n))

**Merge Sorting 归并排序 代码**

```python
#Divide the problem

def MergeSorting(a):
    n = len(a)
    if n == 1:
        return a
    m = n//2
    B = MergeSorting(a[0:m])
    C = MergeSorting(a[m:n])
    Aprime = Merge(B,C)
    return Aprime

#merge the 2 arrays

def Merge(B,C):
    D = []
    while B and C :
        if B[0]<C[0]:
            D.append(B.pop(0))
        else:
            D.append(C.pop(0))
    if C:
        D.extend(C)
    else:
        D.extend(B)
    return D
```
<br>



#### None comparison-based algorithm 

> 以上的几种算法均为comparsion-based algorithms, 需要通过对比数来进行排序。有一种算法叫Count Sort 不需要进行对比，代码如下

**count sorting**

```python
#non-comparison based sorting algorithms

def countSort(A):
    k = max(A)
    b = [0 for i in range(len(A))]
    c = [0 for i in range(k+1)]

    #将A中的元素放入c中

    for j in A:
        c[j] += 1

    #c中，index代表A元素的大小，每个值代表在A中出现的次数

    for i in range(1,len(c)):
        c[i] = c[i] + c[i-1]
    for j in A:
        b[c[j]-1] = j
        c[j] -= 1
    return b
```

<br>

#### 快速排序 Quick Sort

>最后是快速排序*quick sort*, 是一种效率很高的排序方法，不需要占用额外内存，average running time = O(nlog(n)), 最差情况为O(n^2). quick sorting 也是一种 comparsion-base algorithms 

最简单的代码实现如下

**Quick Sorting快速排序**

```python
'''start of the quick sorting algorithm'''
##QuickSorting

def quickSort(A,l,r):
    if l<r:
        m = partition(A,l,r)
        quickSort(A,l,m-1)
        quickSort(A,m+1,r)

#partition algorithms

def partition(A,l,r):
    x = A[l]
    j = l
    for i in range(l+1, r+1):
        if A[i]<=x:
            j+=1
            A[j], A[i] = A[i], A[j]

            #通过 交换，让j左边的数都比x小，j

    A[l], A[j] = A[j], A[l]
    return j

#implement the the quicksort

def quickSortFunc(A):
    quickSort(A,0,len(A)-1)
    return A
'''end of the quick sorting algorithm'''

```
