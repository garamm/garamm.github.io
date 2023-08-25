---
layout: post
title: "[iOS/Swift] UITextView에 Highlight 추가하기"
date: "2022-12-09 00:59"
updated: []
categories: [iOS]
tags: [Highlight]
---

```swift
import UIKit

class ViewController: UIViewController, UITextViewDelegate {
    
    @IBOutlet weak var sampleTextView: UITextView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let searchString = "안녕"
        let baseString = "안녕하세요!"

        let attributed = NSMutableAttributedString(string: baseString)
        do {
           let regex = try! NSRegularExpression(pattern: searchString,options: .caseInsensitive)
           for match in regex.matches(in: baseString, options: NSRegularExpression.MatchingOptions(), range: NSRange(location: 0, length: baseString.count)) as [NSTextCheckingResult] {
               attributed.addAttribute(.backgroundColor, value: UIColor.yellow, range: match.range)
           }
           // 기존 폰트 사이즈 유지를 위해 아래 코드 추가
           attributed.addAttribute(.font, value: sampleTextView.font!, range: (baseString as NSString).range(of: baseString))
           
           self.sampleTextView.attributedText = attributed
        }
    }
    
}
```
<p align="center"><img src="/assets/img/posts/ios-highlight.png" alt="ios-highlight" width="400"></p>