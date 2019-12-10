---
layout: post
title: "[iOS/Swift] 앱에서 사파리로 url 호출"
categories: iOS Swift
---


```swift
let url = URL(string: "https://garamm.github.io")
UIApplication.shared.openURL(url!)
```