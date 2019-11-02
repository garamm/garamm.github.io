---
layout: posts
title: "[iOS/Swift] 전화걸기"
comments: true
categories: [iOS/Swift]
---


```swift
let url = NSURL(string: "tel://01012345678")!
if #available(iOS 10.0, *) {
    UIApplication.shared.open(url as URL)
} else {
    UIApplication.shared.openURL(url as URL)
}
```

