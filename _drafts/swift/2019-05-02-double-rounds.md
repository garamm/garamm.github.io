---
layout: post
title: "[iOS/Swift] 소수점 반올림"
category: swift
date: "2019-05-02"
---


```swift
let numberOfPlaces = 3.0 // 소수점 몇자리에서 반올림할 지 설정
let multiplier = pow(10.0, numberOfPlaces)
let num = 10.12345
let rounded = round(num * multiplier) / multiplier
print(rounded)
// 결과값: 10.123
```
