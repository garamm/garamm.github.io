---
layout: post
title: "[iOS/Swift] 내장 효과음 사용하기"
category: swift
date: "2019-05-02"
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
