---
layout: post
title: "[iOS/Swift] 특수문자 입력 막기"
categories: iOS Swift
---


```swift
// 특수문자, 이모지 입력 막기
// 단, 특수문자 중에 -와 _는 입력 가능하게 만들기

// UITextFieldDelegate 추가하기

@objc func textFieldDidChange(_ textField: UITextField) {
	var value = goodsName.text!
    let lastChar = value.remove(at: goodsName.text!.index(before: goodsName.text!.endIndex))
	print("lastChar ==> \(lastChar)")
	if "\(lastChar)" != "-" && "\(lastChar)" != "_" {
		if !checkString(newText: "\(lastChar)") {
			self.goodsName.text = (goodsName.text!).replacingOccurrences(of: "\(lastChar)", with: "")
		}
	}
}
    
func checkString(newText:String, filter:String = "[a-zA-Z0-9가-힣ㄱ-ㅎㅏ-ㅣ]") -> Bool {
	let regex = try! NSRegularExpression(pattern: filter, options: [])
	let list = regex.matches(in:newText, options: [], range:NSRange.init(location: 0, length:newText.count))
	if(list.count != newText.count) {
		return false
	}
	return true
}
```  