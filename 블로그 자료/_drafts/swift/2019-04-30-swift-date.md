---
layout: post
title: "[iOS/Swift] Date 날짜 관련"
category: swift
date: "2019-04-30"
---


```swift
// 오늘 날짜 구하기
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyyy-MM-dd" // 날짜 형식 지정
var todayDate = Date()
var todayStr = dateFormatter.string(from: todayDate)
print(todayStr)
```



```swift
// 월 구하기
var todayDate = Date()
let calendar = NSCalendar.current
let components = calendar.dateComponents([.day, .month, .year], from: todayDate)
let month = components.month // Int로 return됨
```


```swift
// 어제 날짜 구하기
var todayDate = Date()
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyyy-MM-dd"
var yesterdayDate = Date(timeInterval: -(24 * 60 * 60), since: todayDate)
var yesterdayStr = dateFormatter.string(from: yesterdayDate)
print(yesterdayStr)
```



```swift
// 하루 더하기
var todayDate = Date()
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyyy-MM-dd"
var tomorrowDate = Date(timeInterval: 24 * 60 * 60, since: todayDate)
var tomorrowStr = dateFormatter.string(from: tomorrowDate)
print(tomorrowStr)
```




```swift
// 두 날짜 빼기
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyyy-MM-dd"
let startDate: Date = dateFormatter.date(from: "2018-10-20")!
let endDate: Date = dateFormatter.date(from: "2018-11-21")!

// 방법 1
var diffSec = endDate.timeIntervalSince(startDate)
print("방법 1 결과 : \(Int(diffSec/86400))일 차이")

// 방법 2
let days = Calendar.current.dateComponents([.day], from: startDate, to: endDate)
let diffSec2 = Int(days.day!)
print("방법 2 결과 : \(diffSec2)일 차이")
```



```swift
// 해당 월의 마지막 날짜 구하기
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyyy-MM-dd"
let calendar = NSCalendar.current
let calendar2 = NSCalendar.current
let date2 = dateFormatter.date(from: "2019-04-01")
let components2 = calendar.dateComponents([.year, .month], from: date2!)
let startOfMonth = calendar.date(from: components2)
let comps2 = NSDateComponents()
comps2.month = 1
comps2.day = -1
let endOfMonth = calendar.date(byAdding: comps2 as DateComponents, to: startOfMonth!)
print(dateFormatter.string(from: endOfMonth!))
```




```swift
// 다음날 구하기
var todayDate = Date()
let nextMonth = Calendar.current.date(byAdding: .month, value: 1, to: todayDate)
```