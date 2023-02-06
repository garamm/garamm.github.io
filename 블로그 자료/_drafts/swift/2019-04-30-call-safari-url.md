---
layout: post
title: "[iOS/Swift] 앱에서 사파리로 url 호출"
category: swift
date: "2019-04-30"
---


```swift
let url = URL(string: "https://garamm.github.io")
UIApplication.shared.openURL(url!)
```