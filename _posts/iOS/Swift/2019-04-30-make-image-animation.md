---
layout: post
title: "[iOS/Swift] 이미지 여러개로 애니메이션 만들기"
categories: iOS Swift
---


```swift
var img = [UIImage]() // 이미지 배열 생성

// 이미지 배열 안에 이미지 넣기
img.append(UIImage(named: "ic_ing_00.png")!)
img.append(UIImage(named: "ic_ing_01.png")!)
img.append(UIImage(named: "ic_ing_02.png")!)
img.append(UIImage(named: "ic_ing_03.png")!)
img.append(UIImage(named: "ic_ing_04.png")!)

loadingImage.animationImages = img // 애니메이션 설정!
loadingImage.animationDuration = 1.5 // 애니메이션 시간 설정
loadingImage.startAnimating() // 시작!!
```