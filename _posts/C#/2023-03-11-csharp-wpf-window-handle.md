---
layout: post
title: "[C#, WPF] Window Handle 사용하기"
date: "2023-03-11 21:04"
updated: []
categories: [C#]
tags: [Taskbar]
---

**[1] Visual Studio에서 Spy++ 프로그램 설치**<br>
1\. 도구 → 도구 및 기능 가져오기 선택<br>
<p align="center"><img src="/assets/img/posts/csharp-wpf-window-handle-1.png" alt="csharp-wpf-window-handle-1"></p>
<br>
2\. C++를 사용한 데스크톱 개발 선택하여 다운로드<br>
<p align="center"><img src="/assets/img/posts/csharp-wpf-window-handle-2.png" alt="csharp-wpf-window-handle-2"></p>
<br>
**[2] Spy++ 사용하기**<br>
1\. 도구 → Spy++ 선택<br>
<p align="center"><img src="/assets/img/posts/csharp-wpf-window-handle-3.png" alt="csharp-wpf-window-handle-3"></p>
2\. Ctrl + F 입력<br>
3\. 찾기 도구 선택한 후 원하는 곳에 드래그 하여 핸들값 찾은 후 확인을 누른다.<br>
<p align="center"><img src="/assets/img/posts/csharp-wpf-window-handle-4.png" alt="csharp-wpf-window-handle-4"></p>
4\. 속성 검사자 창이 뜨며 컨트롤 ID 정보를 받아올 수 있다.<br>
컨트롤 ID는 16진수로 되어 있으니 10진수로 변환하여 윈도우 핸들을 사용하면 된다.<br>
<p align="center"><img src="/assets/img/posts/csharp-wpf-window-handle-5.png" alt="csharp-wpf-window-handle-5"></p>