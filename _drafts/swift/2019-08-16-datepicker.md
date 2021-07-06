---
layout: post
title: "[iOS/Swift] DatePicker"
category: swift
date: "2019-08-16"
---

```swift
import UIKit

class ViewController: UIViewController, UITextFieldDelegate {

    var datePicker: UIDatePicker!
    @IBOutlet var resultField: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        resultField.delegate = self
        let now = NSDate()
        let dateFormatter = DateFormatter()
        dateFormatter.dateFormat = "yyyy-MM-dd"
        resultField.text = dateFormatter.string(from: now as Date)
    }
    
    func textFieldDidBeginEditing(_ textField: UITextField) {
        self.pickUpDate(textField)
        datePicker.datePickerMode = .date
        datePicker.locale = NSLocale(localeIdentifier: "ko_KR") as Locale
        textField.inputView = datePicker
        datePicker.addTarget(self, action: #selector(self.datePickerValueChanged), for: UIControl.Event.valueChanged)

        // datePicker에서 선택 가능한 날짜 범위 지정
        // 예시: 오늘 이후로만 검색 가능하게 옵션 추가
        // datePicker.minimumDate = NSDate() as Date
    }
    
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        view.endEditing(true)
    }
    
    @objc func datePickerValueChanged(_ sender: UIDatePicker) {
        let dateFormatter = DateFormatter()
        dateFormatter.dateFormat = "yyyy-MM-dd"
        resultField.text = dateFormatter.string(from: sender.date)
    }
    
    func pickUpDate(_ textField : UITextField){
        
        // DatePicker
        self.datePicker = UIDatePicker(frame:CGRect(x: 0, y: 0, width: self.view.frame.size.width, height: 216))
        //        self.datePicker.backgroundColor = UIColor.white
        self.datePicker.datePickerMode = .date
        textField.inputView = self.datePicker
        
        // ToolBar
        let toolBar = UIToolbar(frame: CGRect(x: 0, y: self.view.frame.size.height/6, width: self.view.frame.size.width, height: 40.0))
        toolBar.layer.position = CGPoint(x: self.view.frame.size.width/2, y: self.view.frame.size.height-20.0)
        //        toolBar.barStyle = .default
        //        toolBar.isTranslucent = true
        //        toolBar.tintColor = UIColor(red: 92/255, green: 216/255, blue: 255/255, alpha: 1)
        //        toolBar.sizeToFit()
        
        // Toolbar의 버튼 설정
        let todayBtn = UIBarButtonItem(title: "오늘 날짜", style: .plain, target: self, action: #selector(self.tappedToolBarBtn))
        let spaceButton = UIBarButtonItem(barButtonSystemItem: .flexibleSpace, target: nil, action: nil)
        let cancelButton = UIBarButtonItem(title: "확인", style: .plain, target: self, action: #selector(self.cancelClick))
        toolBar.setItems([todayBtn, spaceButton, cancelButton], animated: false)
        toolBar.isUserInteractionEnabled = true
        textField.inputAccessoryView = toolBar
    }
    
    @objc func tappedToolBarBtn(_ sender: UIBarButtonItem) {
        let dateformatter = DateFormatter()
        dateformatter.dateFormat = "yyyy-MM-dd"
        resultField.text = dateformatter.string(from: Date())
        view.endEditing(true)
    }
    
    
    @objc func cancelClick() {
        view.endEditing(true)
    }
    
    @IBAction func clickAction(_ sender: Any) {
        
    }
}
```
![img1](/img/2019-08-16-datepicker-1.png)
![img1](/img/2019-08-16-datepicker-2.png)