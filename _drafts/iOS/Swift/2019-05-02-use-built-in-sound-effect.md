---
layout: post
title: "[iOS/Swift] 내장 효과음 사용하기"
categories: iOS Swift
---


```swift
import UIKit
import AVFoundation
import AudioToolbox

class ViewController: UIViewController {

    var audioPlayer : AVAudioPlayer!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        AudioServicesPlaySystemSound(1015)
    }
}
```
