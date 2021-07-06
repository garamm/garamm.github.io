---
layout: post
title: "[iOS/Swift] 음성 권한 허가 여부 확인"
category: swift
date: "2019-05-02"
---


```swift
import UIKit
import AVFoundation

func checkAudioPermission() -> Bool {
    var isAudioRecordingGranted = false
    switch AVAudioSession.sharedInstance().recordPermission {
    case AVAudioSession.RecordPermission.granted:
        isAudioRecordingGranted = true
        break
    case AVAudioSession.RecordPermission.denied:
        isAudioRecordingGranted = false
        break
    case AVAudioSession.RecordPermission.undetermined:
    AVAudioSession.sharedInstance().requestRecordPermission({ (allowed) in
            if allowed {
                isAudioRecordingGranted = true
            } else {
                isAudioRecordingGranted = false
            }
        })
        break
    }
    return isAudioRecordingGranted
}
```
