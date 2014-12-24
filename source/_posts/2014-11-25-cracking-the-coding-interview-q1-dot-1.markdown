---
layout: post
title: "Cracking the coding interview Q1.1"
date: 2014-11-25 18:23:30 +0800
comments: true
categories: [algorithm, ctci, swift]
---
#问题
Q1.1 Implement an algorithm to determine if a string has all unique characters What if you can not use additional data structures?

实现一个算法来判断一个字符串中的字符是否唯一，不能使用额外的数据结构。 (即只使用基本的数据结构)

#解法一：
简单起见我们假设字符串中的字符属于ASCII字符集(其它字符集类似)，那么我们建立一个256元素的Bool数组，并全部初始化为false。遍历字符串中的字符，当字符编码所对应的数组元素为true时，说明存在重复字符，否则将该对应位置的数组元素标记为true。算法时间复杂度为O(n)。

**注：Swift里Char和String均为Unicode，这点和C++不同，所以遍历字符串中的字符时需要使用`for char in str.unicodeScalars`，并且需要将Char类型的value强制转换为Int `Int(char.value)`来对应Bool数组的下标。**

```swift
func isUniqueChar1(str: String) -> Bool {
    var char_set = [Bool](count: 256, repeatedValue: false)
    for char in str.unicodeScalars {
        if char_set[Int(char.value)] == true {
            return false
        }
        char_set[Int(char.value)] = true
    }
    return true
}

isUniqueChar1("abcd")
isUniqueChar1("abcdd")
```

#解法二

解法二与解法一类似，只是将用Bool数组来表示是否出现过重复改成了用位运算，减少了一些空间使用量，本质并没有区别。我使用的MBA2013mid是Core i5 64bit CPU所以`Int = Int64`(Swift同样可以用sizeof()查看，`sizeof(Int) = sizeof(Int64) = 8 bytes`)，256个字符就只需要使用4个Int64来记录。代码如下：

```swift
func isUniqueChar2(str: String) -> Bool {
    var array = [Int](count: 4, repeatedValue: 0) // 4 * 64bit = 256
    for char in str.unicodeScalars {
        var idx: Int = Int(char.value / 64)
        var shift = Int(char.value % 64)
        if (array[idx] & (1 << shift)) > 0 {
            return false
        }
        array[idx] |= (1 << shift)
    }
    return true
}

isUniqueChar2("abcd")
isUniqueChar2("abccd")
```

---

**完整的解题目录：[Solutions for Cracking The Coding Interview Written in Swift]({% post_url 2014-11-23-solutions-for-cracking-the-coding-interview-written-in-swift %})**

**全部代码托管在Github：[https://github.com/WyattZhang/CTCI](https://github.com/WyattZhang/CTCI)**