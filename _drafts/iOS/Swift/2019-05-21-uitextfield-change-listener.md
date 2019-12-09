---
layout: posts
title: "[iOS/Swift] UITextField 입력 이벤트 처리(Listener)"
comments: true
categories: [iOS/Swift]
---

TextField에서 일정 글자수가 채워지면 다음 TextField로 이동하기 예제  
```swift
import UIKit

class ViewController: UIViewController {

    @IBOutlet var field1: UITextField!
    @IBOutlet var field2: UITextField!
    @IBOutlet var field3: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        field1.addTarget(self, action: #selector(self.movePosition(_:)), for: UIControlEvents.editingChanged)
        field2.addTarget(self, action: #selector(self.movePosition(_:)), for: UIControlEvents.editingChanged)
        field3.addTarget(self, action: #selector(self.movePosition(_:)), for: UIControlEvents.editingChanged)
    }
    
    @objc func movePosition(_ textField: UITextField) {
        switch textField {
        case field1:
            if field1.text?.count == 3 {
                field2.becomeFirstResponder()
            }
            break
        case field2:
            if field2.text?.count == 4 {
                field3.becomeFirstResponder()
            }
            break
        case field3:
            if field3.text?.count == 4 {
                view.endEditing(true)
            }
            break
        default:
            break
        }
    }
    
}
```