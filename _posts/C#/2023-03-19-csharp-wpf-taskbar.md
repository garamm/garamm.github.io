---
layout: post
title: "[C#, WPF] 작업표시줄 깜빡이기"
date: "2023-03-19 20:14"
updated: []
categories: [C#]
tags: [Taskbar]
---

```csharp
using System.Windows.Interop;
using System.Runtime.InteropServices;

[DllImport("user32")] public static extern int FlashWindow(IntPtr hwnd, bool bInvert);

...

WindowInteropHelper wih = new WindowInteropHelper(ThisWindow); 
FlashWindow(wih.Handle, true);
```
<br>
<br>
[출처: stack overflow](https://stackoverflow.com/questions/5118226/how-to-make-a-wpf-window-to-blink-on-the-taskbar){:target="_blank"}