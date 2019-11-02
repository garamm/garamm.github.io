---
layout: posts
title: "[iOS/Swift] 네트워크 상태 확인하기(ReachabilitySwift)"
comments: true
categories: [iOS/Swift]
---


1) Podfile 설정
```shell
target 'ReachabilitySwiftExample' do
  # Comment the next line if you're not using Swift and don't want to use dynamic frameworks
  use_frameworks!

  # Pods for ReachabilitySwiftExample
  pod 'ReachabilitySwift'
end
```
2) Framework가 잘 설정됐는지 확인
![img1](/img/2019-05-31-check-network-state-1.png)  
  
3) info.plist에서 인터넷 허용  
![img1](/img/2019-05-31-check-network-state-2.png)  
  
4) 코드 작성
```swift
// ViewController.swift
import UIKit
import Reachability

class ViewController: UIViewController {
    
    var alertController: UIAlertController!
    let reachability = Reachability()!

    override func viewDidLoad() {
        super.viewDidLoad()
        
        NotificationCenter.default.addObserver(self, selector: #selector(reachabilityChanged(note:)), name: .reachabilityChanged, object: reachability)
        do{
            try reachability.startNotifier()
        }catch{
            print("could not start reachability notifier")
        }
        
    }

    @objc func reachabilityChanged(note: Notification) {
        
        let reachability = note.object as! Reachability
        
        switch reachability.connection {
        case .wifi:
            print("Reachable via WiFi")
            if alertController != nil {
                alertController.dismiss(animated: true, completion: nil)
                alertController = nil
            }
        case .cellular:
            print("Reachable via Cellular")
            if alertController != nil {
                alertController.dismiss(animated: true, completion: nil)
                alertController = nil
            }
        case .none:
            print("Network not reachable")
            self.alertController = UIAlertController(title: "알림", message: "네트워크 연결실패\n다시 시도 하시겠습니까?", preferredStyle: UIAlertController.Style.alert)
            let okAction = UIAlertAction(title: "확인", style: UIAlertAction.Style.default) {
                (action: UIAlertAction) in
                self.reachabilityChanged(note: note)
            }
            self.alertController.addAction(okAction)
            self.present(alertController, animated: true, completion: nil)
        }
    }
    
    override func viewDidDisappear(_ animated: Bool) {
        reachability.stopNotifier()
        NotificationCenter.default.removeObserver(self, name: .reachabilityChanged, object: reachability)
    }

}
```