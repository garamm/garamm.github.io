---
layout: post
title: "[iOS/Swift] 문자열에서 숫자만 추출하기"
category: swift
date: "2019-04-30"
---


```swift
let str = "12abc3d456ab7c8dwadad9"
let intString = str.components(separatedBy: 
        CharacterSet.decimalDigits.inverted).joined(separator: "")
print(intString)
```

