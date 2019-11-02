---
layout: posts
title: "[iOS/Swift] UILabel, UIView, UIImageView에 터치 액션 추가하기"
comments: true
categories: [iOS/Swift]
---


```swift
// viewDidLoad() 안에 다음과 같이 작성
label.isUserInteractionEnabled = true
// ㄴ 일반 뷰는 버튼과 다르게 addTarget이 없으므로 요걸 반드시 해주셔야 합니다.
// UITableView 안에 있는 뷰에 액션을 추가할 땐 태그 추가를 다음과 같이 함
label.tag = indexPath.row
label.addGestureRecognizer(UITapGestureRecognizer(target: self, action: #selector(touchAction)))



// viewDidLoad() 밖에는 다음과 같이 작성
@objc func touchAction(sender: UITapGestureRecognizer) {
        let idx = sender.view?.tag
        print("TOUCH!!!")
}
```