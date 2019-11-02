---
layout: posts
title: "[iOS/Swift] substring 문자열 자르기"
comments: true
categories: [iOS/Swift]
---


```swift
let exampleStr = "안녕하세요. 이것은 substring 예제입니다."
let startIndex = exampleStr.index(exampleStr.startIndex, offsetBy: 7) // 시작위치 7
let endIndex = exampleStr.index(startIndex, offsetBy: 3) // 길이 3
let substring = exampleStr[startIndex..<endIndex]
print(substring)
```