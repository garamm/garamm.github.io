---
layout: post
title: "[iOS/Swift] 인터넷 권한 추가하기"
date: "2023-03-17 01:31"
updated: []
categories: [iOS]
tags: [permission]
---

1\. **info.plist** 파일 열기<br>
2\. **Information Property List** 아래에 **App Transport Security Settings** 추가<br>
3\. **App Transport Security Settings** 아래에 **Allow Arbitrary Loads** 추가<br>
4\. **Allow Arbitrary Loads**의 Value를 **YES**로 변경
<p align="center"><img src="/assets/img/posts/ios-internet-permission.png" alt="ios-internet-permission"></p>