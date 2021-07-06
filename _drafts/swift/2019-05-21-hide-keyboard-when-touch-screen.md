---
layout: post
title: "[iOS/Swift] 키보드 숨기기"
category: swift
date: "2019-05-21"
---
   

방법 1  
```swift
extension UIViewController {
    func hideKeyboardWhenTappedAround() {
        let tap: UITapGestureRecognizer = UITapGestureRecognizer(target: self, action: #selector(UIViewController.dismissKeyboard))
        tap.cancelsTouchesInView = false            
        view.addGestureRecognizer(tap)
    }

    @objc func dismissKeyboard() {
        view.endEditing(true)
    }
}


override func viewDidLoad() {
    super.viewDidLoad()
    self.hideKeyboardWhenTappedAround() 
}
```

방법 2   
```swift
override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?){
      self.view.endEditing(true)
}
```