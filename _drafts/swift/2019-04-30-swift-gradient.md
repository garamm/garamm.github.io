---
layout: post
title: "[iOS/Swift] Gradient"
category: swift
date: "2019-04-30"
---


```swift
class ViewController: UIViewController {

    @IBOutlet var myView: UIView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let gradient = CAGradientLayer()
        gradient.frame = CGRect(origin: .zero, size: self.myView.frame.size)
        gradient.colors = [UIColor.startColor.cgColor, UIColor.endColor.cgColor]
        
        let shape = CAShapeLayer()
        shape.lineWidth = myView.frame.size.width/2
        shape.path = UIBezierPath(rect: self.myView.bounds).cgPath
        shape.strokeColor = UIColor.black.cgColor
        shape.fillColor = UIColor.clear.cgColor
        gradient.mask = shape
        
        self.myView.layer.addSublayer(gradient)

    }
}


extension UIColor {
    
    static let startColor = UIColor(hex: 0xDBEDFF)
    static let endColor = UIColor(hex: 0xEFFAF9)
    
    convenience init(hex: Int, a: CGFloat = 1.0) {
        self.init(
            red: (hex >> 16) & 0xFF,
            green: (hex >> 8) & 0xFF,
            blue: hex & 0xFF,
            a: a
        )
    }
}
```

![img1](/img/2019-04-30-swift-gradient-1.png)