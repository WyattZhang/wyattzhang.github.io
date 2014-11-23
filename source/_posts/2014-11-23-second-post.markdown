---
layout: post
title: "Second Post"
date: 2014-11-23 01:18:30 +0800
comments: true
categories: 
---

Test
=========
``` c
void
```

作为可视化开发的工具，IB 和 Storyboard 在组织和构建 ViewController 及其导航关系时已经做得很好的。对于 ViewController 的 view 画布上的诸如 UILabel 或者 UIImageView 这样的基础的类，IB 是能够很好地支持并实时在设计的时候进行显示的。但是对于那些自定义的类，之前的 IB 就束手无策了。我们能做的仅仅是在 IB 中拖放一个 UIView，然后通过将 Custom Class 属性设置为我们自定义的 UIView 的子类来在 “暗示” IB 在运行时初始化一个对应的子类。这样的问题是在开发自定义的 view 时，我们不得不一遍遍地修改代码并运行，再根据运行结果进行调整和修正。而实际上，单一对某个 view 的调试这种问题只涉及到设计层面，而非运行层面，如果我们能够在设计时就有一个实时地对自定义 view 的预览该多好。

没错，Apple 也是这么想的，并且在 Xcode 6 中，我们就已经可以创建这样的 UIView 子类了：利用新加入的 @IBDesignable 和 @IBInspectable，我们可以非常方便地完成在 IB 中实时显示自定义视图，甚至和其他一些内置 UIView 子类一样，直接在 IB 的 Inspector 改变某些属性，甚至我们还能通过设置断点来在 IB 中显示视图时进行调试。新的这些特性非常强大，使用起来却出乎意料的简单。下面我将通过一个实际的小例子加以说明。最终的完整例子已经放在 GitHub 上了，现在我们从开始一步步开始吧。这些代码基于 Xcode 6.1 和 Swift 1.1。

``` swift
//1.3 Design an algorithm and write code to remove the duplicate characters in a string without using any additional buffer NOTE: One or two additional variables are fine An extra copy of the array is not
//  Write the test cases for this method

func removeDuplicates(var str: [Character]) -> [Character] {
let strLen = str.count
if strLen < 2 {
return str
}
var tail = 1
for i in 1..<strLen {
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


for var x = 0; x < str.count; ++x {     // Remove characters after "\0"
if str[x] == "\0" {
let range = Range(start: x, end: str.count)
str.removeRange(range)
break
}
}

return str
}

removeDuplicates(Array("abcd"))
removeDuplicates(Array("aaaa"))
removeDuplicates(Array(""))
removeDuplicates(Array("aaabbbcc"))
removeDuplicates(Array("abcabcabc"))
```

