---
layout: post
title: "[iOS/Swift] 배경, 테두리에 그라데이션 넣기"
date: "2019-04-30"
updated: []
categories: [iOS]
tags: [Gradient]
---

**1. 배경에 그라데이션 넣기**
```swift
let gradient = CAGradientLayer()
gradient.frame = CGRect(origin: .zero, size: imgView.frame.size)
let startColor = UIColor.yellow.cgColor
let endColor = UIColor.green.cgColor
gradient.colors = [startColor, endColor]

imgView.layer.addSublayer(gradient)
```
<p align="center"><img src="/assets/img/posts/ios-gradient-1.png" alt="ios-gradient-1"></p>
<br>
**2. 테두리 그라데이션**
```swift
let gradient = CAGradientLayer()
gradient.frame = CGRect(origin: .zero, size: imgView.frame.size)
let startColor = UIColor.yellow.cgColor
let endColor = UIColor.green.cgColor
gradient.colors = [startColor, endColor]

/* 추가 부분 시작 */
let shape = CAShapeLayer()
shape.lineWidth = imgView.frame.size.width/2
shape.path = UIBezierPath(rect: imgView.bounds).cgPath
shape.strokeColor = UIColor.black.cgColor
shape.fillColor = UIColor.clear.cgColor
gradient.mask = shape
/* 추가 부분 끝 */

imgView.layer.addSublayer(gradient)
```
<p align="center"><img src="/assets/img/posts/ios-gradient-2.png" alt="ios-gradient-2"></p>