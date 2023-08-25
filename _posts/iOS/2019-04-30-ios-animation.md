---
layout: post
title: "[iOS/Swift] 이미지 여러개로 애니메이션 만들기"
date: "2019-04-30"
updated: []
categories: [iOS]
tags: [Animation]
---

```swift
var imgs = [UIImage]() // 이미지 배열 생성
// 이미지 배열에 이미지 추가
for i in 1..<33 {
	let imgName = "loading_\(i).png"
    imgs.append(UIImage(named: imgName)!)
}
imgView.animationImages = imgs // 애니메이션 이미지 설정
imgView.animationDuration = 1.5 // 애니메이션 실행 시간 설정
imgView.startAnimating() // 애니메이션 시작
```
<p align="center"><img src="/assets/img/posts/ios-animation.gif" alt="ios-animation" width="400"></p>