---
layout: post
title: "[iOS/Swift] Delay"
category: swift
date: "2019-04-30"
---


```swift
DispatchQueue.main.asyncAfter(deadline: DispatchTime.now() + .seconds(1), execute: {
    // delay 이후 진행할 작업 작성
    // 1초 딜레이 준 코드입니다.
})
```