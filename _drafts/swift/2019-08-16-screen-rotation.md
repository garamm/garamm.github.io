---
layout: post
title: "[iOS/Swift] 화면 회전"
category: swift
date: "2019-08-16"
---

특정 ViewController에서 화면 회전
```swift
// 화면 자동 회전 금지
override var shouldAutorotate: Bool {
	get {
		return false
	}
}
    
// 화면 가로/세로로 고정
override var supportedInterfaceOrientations: UIInterfaceOrientationMask {
	get {
	// return .landscape
	return .portrait
	}
}
```

전체적으로 화면 회전 설정
Project > General > Deployment Info > Device Orientation
![img1](/img/2019-08-16-screen-rotation-1.png)
