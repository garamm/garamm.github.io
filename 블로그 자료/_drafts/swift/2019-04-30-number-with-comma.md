---
layout: post
title: "[iOS/Swift] 숫자에 콤마 찍기, 없애기"
category: swift
date: "2019-04-30"
---


```swift
extension Int {
    var addComma: String {
        let decimalFormatter = NumberFormatter()
        decimalFormatter.numberStyle = NumberFormatter.Style.decimal
        decimalFormatter.groupingSeparator = ","
        decimalFormatter.groupingSize = 3
        
        return decimalFormatter.string(from: self as NSNumber)!
    }
}

extension String {
    var removeComma: String {
        if self.characters.count > 0 {
            return self.replacingOccurrences(of: ",", with: "")
        } else {
            return "0"
        }
    }
}
```



```swift
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        let a: String = (123456789).addComma
        let b: String = "123,45a".removeComma
        
        print(a)
        print(b)
        
    }
}
```
