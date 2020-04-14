---
layout: post
title: "[iOS/Swift] 카카오링크 - 텍스트템플릿 사용하기"
categories: iOS Swift
---

```swift
let title = "제목입니다."
let template = KMTTextTemplate.init{ (textTemplate) in
    textTemplate.text = title
    textTemplate.buttonTitle = "버튼타이틀입니다."
    textTemplate.link = KMTLinkObject(builderBlock: { (linkBuilder) in
        linkBuilder.mobileWebURL = URL(string: self.url)!
    })
}
        
KLKTalkLinkCenter.shared().sendDefault(with: template, 
    success: {(warningMsg, argumentMsg) in
        print("warning message: \(warningMsg)")
        print("argument message: \(argumentMsg)")
    }, failure: {(error) in
        print("error: \(error)")
    }
)
```
