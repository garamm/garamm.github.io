---
layout: post
title: "특정 모서리에 radius 추가하기"
category: swift
date: "2019-04-30"
---


```swift
imageView.clipsToBounds = true
imageView.layer.cornerRadius = 10

// 오른쪽 위, 왼쪽 위 라운드
//topView.layer.maskedCorners = [.layerMaxXMinYCorner, .layerMinXMinYCorner]

// 오른쪽 아래, 왼쪽 아래 라운드
imageView.layer.maskedCorners = [.layerMinXMaxYCorner, .layerMaxXMaxYCorner]
```

![img1](/img/2019-04-30-add-radius-1.png)