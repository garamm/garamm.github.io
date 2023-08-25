---
layout: post
title: "[C#, WPF] 딜레이 준 후 작업 처리하기"
date: "2023-04-02 20:57"
updated: []
categories: [C#]
tags: [delay]
---

```csharp
this.Dispatcher.Invoke((ThreadStart)(() => { }), DispatcherPriority.ApplicationIdle);
Thread.Sleep(2000);
// 처리할 작업 작성
```
<br>
<br>
[dr. Bee Eye: C# WPF delay 주는 방법](https://drbeeeye.tistory.com/28){:target="_blank"}
