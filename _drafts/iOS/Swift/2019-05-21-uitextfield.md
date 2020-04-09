---
layout: post
title: "[iOS/Swift] UITextField"
categories: iOS Swift
---


---
특정 UITextField에 포커스가 있는지 확인, return true/false
```swift
// textFieldShouldReturn에서 UITextField마다 return 이벤트를 다르게 주고싶을 때 사용
func textFieldShouldReturn(_ textField: UITextField) -> Bool {
    if textField == myTextField {
        let haveFocus = textField.isFirstResponder
        if(haveFocus) {
            print("다음 UITextField로 이동")
        }
    }
    return true
}
```
---
특정 UITextField에 포커스 주기
```swift
textField.becomeFirstResponder()
```
---
UITextField에 입력된 텍스트의 글자수 얻기
```swift
textField.text!.count
```
---
UITextField의 높이를 Auto Layout으로 지정해도 적용이 안될 때
![img1](/img/2019-05-21-uitextfield-1.png)   

---
UITextField에 붙혀넣기 막기
```swift
import Foundation
import UIKit

class CustomUITextField: UITextField {
    override func canPerformAction(_ action: Selector, withSender sender: Any?) -> Bool {
        if action == "paste:" {
            return false
        }
        return super.canPerformAction(action, withSender: sender)
    }
}
```
![img2](/img/2019-05-21-uitextfield-2.png)   
