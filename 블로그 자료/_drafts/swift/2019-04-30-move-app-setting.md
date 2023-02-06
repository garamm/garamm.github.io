---
layout: post
title: "[iOS/Swift] 앱 설정으로 이동하기"
category: swift
date: "2019-04-30"
---


```swift
// 주로 권한이 없는 경우 권한 요청을 다시 하기 위해 사용
UIApplication.shared.open(NSURL(string: 
    "\(UIApplicationOpenSettingsURLString)com.example.MyExample")! as URL)
// com.example.MyExample : Bundle Identifier 명
```