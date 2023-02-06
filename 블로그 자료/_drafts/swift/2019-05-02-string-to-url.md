---
layout: post
title: "[iOS/Swift] String을 URL로 변환한 결과가 nil일 때"
category: swift
date: "2019-05-02"
---


```swift
let urlStr = "https://garamm.github.io/Archive/"
if let encoded = urlStr.addingPercentEncoding(withAllowedCharacters: .urlFragmentAllowed), 
        let myURL = URL(string: encoded) {
    print(myURL)
}
```
