---
layout: post
title: "[iOS/Swift] substring 문자열 자르기"
date: "2019-04-30"
updated: []
categories: [iOS]
tags: [substring]
---

```swift
let str = "안녕하세요. substring 예제입니다."
let startIndex = str.index(str.startIndex, offsetBy: 7) // 시작위치: 7
let endIndex = str.index(startIndex, offsetBy: 9) // 길이: 9
let substring = str[startIndex..<endIndex]
print(substring) // 출력: substring
```