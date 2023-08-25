---
layout: post
title: "[iOS/Swift] UILabel, UIView, UIImageView에 터치 액션 추가하기"
date: "2019-04-30"
updated: []
categories: [iOS]
---

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    
    // 일반 뷰는 버튼과 다르게 addTarget이 없으므로 옵션 추가
    label.isUserInteractionEnabled = true
    label.tag = indexPath.row
	label.addGestureRecognizer(UITapGestureRecognizer(target: self, action: #selector(touchAction)))
    
}

@objc func touchAction(sender: UITapGestureRecognizer) {
	let idx = sender.view?.tag
    ...
}
```