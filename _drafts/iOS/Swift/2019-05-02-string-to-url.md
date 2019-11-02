---
layout: posts
title: "[iOS/Swift] String을 URL로 변환한 결과가 nil일 때"
comments: true
categories: [iOS/Swift]
---


```swift
let urlStr = "https://garamm.github.io/Archive/"
if let encoded = urlStr.addingPercentEncoding(withAllowedCharacters: .urlFragmentAllowed), 
        let myURL = URL(string: encoded) {
    print(myURL)
}
```
