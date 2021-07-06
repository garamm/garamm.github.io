---
layout: post
title: "[iOS/Swift] 전화걸기"
category: swift
date: "2019-04-30"
---


```swift
let url = NSURL(string: "tel://01012345678")!
if #available(iOS 10.0, *) {
    UIApplication.shared.open(url as URL)
} else {
    UIApplication.shared.openURL(url as URL)
}
```

