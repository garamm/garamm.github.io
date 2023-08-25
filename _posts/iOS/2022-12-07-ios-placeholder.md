---
layout: post
title: "[iOS/Swift] UITextView에 PlaceHolder 추가하기"
date: "2022-12-07 12:15"
updated: []
categories: [iOS]
tags: [PlaceHolder]
---

```swift
import UIKit

class ViewController: UIViewController, UITextViewDelegate {
    
    @IBOutlet weak var sampleTextView: UITextView!
    let placeHolderColor = UIColor.gray
    let textColor = UIColor.black
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        sampleTextView.delegate = self
        setPlaceHolder(textView: sampleTextView)
    }
    
    // 텍스트가 입력되기 전 텍스트 색이 PlaceHolder 색이면 입력 색상으로 변경
    func textViewDidBeginEditing(_ textView: UITextView) {
        if textView.textColor == placeHolderColor {
            setNormalText(textView: textView)
        }
    }
    
    // 텍스트가 입력된 후 내용이 비어있으면 PlaceHolder 처리
    func textViewDidEndEditing(_ textView: UITextView) {
        if textView.text.isEmpty {
            setPlaceHolder(textView: textView)
        }
    }
    
    func setPlaceHolder(textView: UITextView) {
        textView.text = "내용을 입력해주세요."
        textView.textColor = placeHolderColor
    }
    
    func setNormalText(textView: UITextView) {
        textView.text = nil
        textView.textColor = textColor
    }

}
```
<p align="center"><img src="/assets/img/posts/ios-placeholder.gif" alt="ios-placeholder" width="400"></p>