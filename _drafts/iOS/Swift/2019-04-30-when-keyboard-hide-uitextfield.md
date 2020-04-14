---
layout: post
title: "[iOS/Swift] 키보드가 UITextField를 가릴 때"
categories: iOS Swift
---


```swift
import UIKit

class ViewController: UIViewController, UITextFieldDelegate {

    @IBOutlet var textField: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        textField.delegate = self
        
        NotificationCenter.default.addObserver(self,
            selector: #selector(keyboardWillShow),
            name: UIResponder.keyboardWillShowNotification, object: nil)
        NotificationCenter.default.addObserver(self,
            selector: #selector(keyboardWillHide),
            name: UIResponder.keyboardWillHideNotification, object: nil)
        
    }

    @objc func keyboardWillShow(_ sender:Notification){
        self.view.frame.origin.y = -200
        // delegate를 사용하기 때문에 모든 UITextField에 적용된다.
        // 만약 일부 UITextField는 적용하고 싶지 않다면
        // (주로 최상위에 있는 UITextField, 최상위에 있는데 높이를 올려버리면 그 UITextField가 status bar에 가려지기 때문)
        // 다음 코드를 작성한다.
        /*
        if !topField.isFirstResponder {
            self.view.frame.origin.y = -150
        }
        */

        // 키보드 높이만큼 올리고 싶다면 다음 코드를 작성한다.
        /*
        guard let info = notification.userInfo else { return }
        guard let frameInfo = info[UIResponder.keyboardFrameEndUserInfoKey] as? NSValue else { return }
        let keyboardFrame = frameInfo.cgRectValue.height
        self.view.frame.origin.y = 0 - keyboardFrame
        */
    }
    
    @objc func keyboardWillHide(_ sender:Notification){
        self.view.frame.origin.y = 0
    }
    
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?){
        self.view.endEditing(true)
    }
    
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        textField.resignFirstResponder()
        return true
    }

}
```

![img1](/img/2019-04-30-when-keyboard-hide-uitextfield-1.png)
![img2](/img/2019-04-30-when-keyboard-hide-uitextfield-2.jpeg)