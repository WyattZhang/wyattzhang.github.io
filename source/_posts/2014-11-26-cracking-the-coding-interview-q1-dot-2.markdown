---
layout: post
title: "Cracking the coding interview Q1.2"
date: 2014-11-26 15:24:36 +0800
comments: true
categories: [algorithm, ctci, swift]
---
#问题
Q1.2 Write code to reverse a C-Style String. (C-String means that “abcd” is represented as five characters, including the null character.)

#解法一：
备注: 由于Swift的String类型结束没有'\0'，为了模拟C Style String，使用了'N'来代替,下同。

```swift
func reverse1(str: String) -> String {
    var end = 0
    var revStr = ""
    for char in str {
        ++end
    }
    --end
    for ; end > 0; end-- {
        revStr.append(Array(str)[end - 1])
    }
    revStr.append("N" as Character)
    return revStr
}
```
#解法二：
在Swift中String类型是传值而非传引用，解法二使用了Swift中的Character数组来模拟C-Style的Char Array。

```swift
func reverse2(var str: [Character]) -> [Character] {
    if str.count < 2 {
        return str
    }
    var start = 0
    var end = str.count - 1 - 1
    for ; start < end; end-- {
//        var tmp = str[start]         // standard swap func
//        str[start] = str[end]
//        str[end] = tmp
        (str[start], str[end]) = (str[end], str[start])  // use tuple in swift for swapping
        start++
    }
    return str
}
```
#解法三：
使用递归

```swift
func reverse3(var str: [Character]) -> [Character] {
    var revStr = [Character]()
    for var i = 0; i < str.count - 1; i++ {
        revStr = "\(str[i])" + revStr
    }
    return revStr + "N"
}
```
测试用例

```swift
reverse("abcd12345N")    // built-in Swift function
reverse1("abcd12345N")
reverse2("abcd12345N")
reverse3("abcd12345N")
```
---

**完整的解题目录：[Solutions for Cracking The Coding Interview Written in Swift]({% post_url 2014-11-23-solutions-for-cracking-the-coding-interview-written-in-swift %})**

**全部代码托管在Github：[https://github.com/WyattZhang/CTCI](https://github.com/WyattZhang/CTCI)**














