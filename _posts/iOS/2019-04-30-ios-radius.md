---
layout: post
title: "[iOS/Swift] 특정 모서리에 radius 추가하기"
date: "2019-04-30"
updated: []
categories: [iOS]
tags: [Gradient]
---

```swift
firstImg.clipsToBounds = true
firstImg.layer.cornerRadius = 10
// 오른쪽 위, 왼쪽 위 라운드
firstImg.layer.maskedCorners = [.layerMaxXMinYCorner, .layerMinXMinYCorner]

secondImg.clipsToBounds = true
secondImg.layer.cornerRadius = 10
// 오른쪽 아래, 왼쪽 아래 라운드
secondImg.layer.maskedCorners = [.layerMinXMaxYCorner, .layerMaxXMaxYCorner]
```
<p align="center"><img src="/assets/img/posts/ios-radius.png" alt="ios-radius"></p>