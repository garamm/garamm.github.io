---
layout: post
title: "[iOS/Swift] 전화걸기"
date: "2019-04-30"
updated: []
categories: [iOS]
tags: [Call]
---

```swift
let url = NSURL(string: "tel://01012345678")!
if #available(iOS 10.0, *) {
	UIApplication.shared.open(url as URL)
} else {
	UIApplication.shared.openURL(url as URL)
}
```
<p align="center"><img src="/assets/img/posts/ios-phone-call.png" alt="ios-phone-call" width="400"></p>