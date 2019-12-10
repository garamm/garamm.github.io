---
layout: post
title: "[iOS/Swift] 문자열에 특정 문자가 들어가는지 확인하고 싶을 때(contains)"
categories: iOS Swift
---


```swift
extension String {

    func contains(find: String) -> Bool{
        return self.range(of: find) != nil
    }

    func containsIgnoringCase(find: String) -> Bool{
        return self.range(of: find, options: .caseInsensitive) != nil
    }

}
```

```swift
// 사용방법
if myLabel.text?.contains(find: "*") {
	// * 이 포함되어 있을 때
} else {
	// * 이 포함되어 있지 않을 때
}
```