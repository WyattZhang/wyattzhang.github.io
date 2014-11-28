---
layout: post
title: "Quicksort written in swift"
date: 2014-11-24 15:20:40 +0800
comments: true
categories: [algorithm, swift]
---
Quicksort是最常用的一种排序算法，很多语言内置的sort函数都是使用quicksort算法(或其改进，如Java7)。

>From Wikipedia : [Quicksort](http://en.wikipedia.org/wiki/Quicksort)

>Quicksort in action on a list of numbers. The horizontal lines are pivot values.
Visualization of the quicksort algorithm. The horizontal lines are pivot values.
Class	Sorting algorithm
Worst case performance	O(n^2)
Best case performance	O(nlogn) (simple partition)
or O(n) (three-way partition and equal keys)
Average case performance	O(nlogn)
Worst case space complexity	O(n) auxiliary (stable)
O(logn) auxiliary (in-place)
Quicksort, or partition-exchange sort, is a sorting algorithm developed by Tony Hoare that, on average, makes O(nlogn) comparisons to sort n items. In the worst case, it makes O(n^2) comparisons, though this behavior is rare. Quicksort is often faster in practice than other O(nlogn) algorithms. Additionally, quicksort's sequential and localized memory references work well with a cache. Quicksort is a comparison sort and, in efficient implementations, is not a stable sort. Quicksort can be implemented with an in-place partitioning algorithm, so the entire sort can be done with only O(logn) additional space used by the stack during the recursion.

一个标准版本的quicksort代码如下：
```swift
// Standard version quicksort
func partition(inout dataList: [Int], low: Int, high: Int) -> Int {
    var pivotPos = low
    var pivot = dataList[low]
    
    for var i = low + 1; i <= high; i++ {
        if dataList[i] < pivot && ++pivotPos != i {
            swap(&dataList[pivotPos], &dataList[i])
        }
    }
    swap(&dataList[low], &dataList[pivotPos])
    return pivotPos
}

func quickSort(inout dataList: [Int], left: Int, right: Int) {
    if left < right {
        var pivotPos = partition(&dataList, left, right)
        quickSort(&dataList, left, pivotPos - 1)
        quickSort(&dataList, pivotPos + 1, right)
    }
}

var dataList = [42, 12, 88, 62, 63, 56, 1, 77, 88, 97, 97, 20, 45, 91, 62, 2, 15, 31, 59, 5]
quickSort(&dataList, 0, dataList.count - 1)
```
Swift语言引入了泛型，下面是一个泛型版本的quicksort代码：
(注：Swift中任何conform `Comparable ` protocol的类型至少要重载`<`和`==`)
```swift
// Generic version quickSort
func partition<T: Comparable>(inout dataList: [T], low: Int, high: Int) -> Int {
    var pivotPos = low
    var pivot = dataList[low]
    for var i = low + 1; i <= high; i++ {
        if dataList[i] < pivot && ++pivotPos != i {
            swap(&dataList[pivotPos], &dataList[i])
        }
    }
    swap(&dataList[low], &dataList[pivotPos])
    return pivotPos
}

func quickSort<T: Comparable>(inout dataList: [T], left: Int, right: Int) {
    if left < right {
        var pivotPos = partition(&dataList, left, right)
        quickSort(&dataList, left, pivotPos - 1)
        quickSort(&dataList, pivotPos + 1, right)
    }
}

struct Vector: Comparable {
    var x: Double = 0.0
    var y: Double = 0.0
}

func <(p: Vector, q: Vector) -> Bool {
    return p.x < q.x && p.y < q.y
}

func ==(p: Vector, q:Vector) -> Bool {
    return p.x == q.x && p.y == q.y
}

var numbers = [42, 12, 88, 62, 63, 56, 1, 77, 88, 97, 97, 20, 45, 91, 62, 2, 15, 31, 59, 5]
var strings = ["b", "c", "Z", "a", "A", " ", ">", "89", "1", "y", "Y", "&"]
var vectors = [Vector(x: 2.0, y: 2.0), Vector(x: 3.0, y: 3.0), Vector(x: 1.0, y: 1.0)]


quickSort(&numbers, 0, numbers.count - 1)
quickSort(&strings, 0, strings.count - 1)
quickSort(&vectors, 0, vectors.count - 1)
```







