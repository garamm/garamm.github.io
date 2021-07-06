---
layout: post
title: "[iOS/Swift] Xcode Error"
category: swift
date: "2019-05-21"
---
   
---
`1. Apple Mach-O Linker Error`   
외부장비(블루투스 프린터기 등)와의 연결이 필요한 경우 기기를 연결해야만 빌드할 수 있음  
.xcworkspace로 프로젝트 열기  
외부 모듈을 사용하는 곳에 import를 정확히 했는지 확인  
외부 모듈을 사용하는 곳에 import를 중복으로 선언했는지 확인  
시뮬레이터가 아닌 실제 기기를 연결하여 실행  
  
  
---
`2. 이미지는 알파 채널 또는 투명도를 포함할 수 없습니다`   
앱 아이콘 이미지를 변경할 때 발생  
Assets.xcassets에 앱 아이콘을 설정할 때 투명도가 포함된 png 파일은 적용할 수 없음
그러므로 jpeg 파일로 변환하여 업로드해야 함
  
  
---
`3. whose view is not in the window hierarchy!`   
기존에 열려있는 firstViewController 를 Dismiss 하고 secondViewController를 Present 하기 위해
```swift
self.dismiss(animated: true, completion: { 
  let vc = self.storyboard?.instantiateViewController(withIdentifier: "secondViewController") 
  self.present(vc!, animated: true, completion: nil) 
})
```
이렇게 코드를 작성한 경우 'whose view is not in the window hierarchy!' 라는 오류가 발생한다.  
이때는 뷰계층구조상 rootViewController를 사용해서 다음과 같이 작성하면 firstViewController는 닫히고, secondViewController가 열린다.   
```swift
self.dismiss(animated: true, completion: {
   let vc = self.storyboard?.instantiateViewController(withIdentifier: "secondViewController")
   let appDelegate = UIApplication.shared.delegate as! AppDelegate
   appDelegate.window?.rootViewController!.present(vc, animated: true, completion: nil) 
})
```
---
`4. [!] Unable to find a specification for ...`  
cocoa pod에서 외부 모듈을 import할 때 발생하는 에러  
```
# pod update
```
pod를 업데이트 한 후 다시 pod install을 하면 제대로 import툄