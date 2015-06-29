---
layout: post
title: "Cracking the coding interview Q1.3"
date: 2014-11-27 15:05:38 +0800
comments: true
categories: [algorithm, ctci, swift]
---
#问题
Q1.3 Design an algorithm and write code to remove the duplicate characters in a string without using any additional buffer NOTE: One or two additional variables are fine An extra copy of the array is not.

FOLLOW UP

Write the test cases for this method.

#解法一：
从第二个字符开始比较，如果和前面已存在的字符相同，就把该字符置空，最后截掉空字符及其后续字符，时间复杂度为O(n^2)。代码如下：

```swift
func removeDuplicates1(inout str: [Character]) {
    let strLen = str.count
    if strLen < 2 {
        return
    }
    var tail = 1
    for i in 1 ..< strLen {
        var j: Int
        for j = 0; j < tail; ++j {
            if str[i] == str[j] {
                break
            }
        }
        if j == tail {
            str[tail] = str[i]
            ++tail
        }
    }
    if tail < str.count {
        str[tail] = "\0"
    }

   for var x = 0; x < str.count; ++x {    
        if str[x] == "\0" {
            let range = Range(start: x, end: str.count)
            str.removeRange(range)
            break
        }
    }   
}
```
测试用例：注:`Array("string")`的作用是将string转换为Character数组

```swift
var testCase1_1 = Array("abcd")
var testCase1_2 = Array("AAaaaa")
var testCase1_3 = Array("")
var testCase1_4 = Array("aaabbbcc")
var testCase1_5 = Array("abcabcabc")

removeDuplicates1(&testCase1_1)
removeDuplicates1(&testCase1_2)
removeDuplicates1(&testCase1_3)
removeDuplicates1(&testCase1_4)
removeDuplicates1(&testCase1_5)

testCase1_1
testCase1_2
testCase1_3
testCase1_4
testCase1_5
```
---

**完整的解题目录：[Solutions for Cracking The Coding Interview Written in Swift]({% post_url 2014-11-23-solutions-for-cracking-the-coding-interview-written-in-swift %})**

**全部代码托管在Github：[https://github.com/WyattZhang/CTCI](https://github.com/WyattZhang/CTCI)**