---
layout: post
title: "[iOS/Swift] Storyboard에서 UIScrollView 사용하는 방법"
date: "2022-10-27 19:26"
updated: []
categories: [iOS]
tags: [ScrollView]
---

**[1] Storyboard에서 보기 쉽게 UIViewController의 높이를 조절한다.**<br>
- UIViewController선택<br>
- Simulated Size를 Fixed에서 Freeform으로 변경<br>
- Height를 원하는 크기로 조정<br>
<p align="center"><img src="/assets/img/posts/ios-scrollview-1.png" alt="ios-scrollview-1"></p>
<br>
**[2] UIScrollView를 배치한다.**<br>
UIScrollView의 상하좌우를 0, 0, 0, 0으로 설정<br>
<p align="center"><img src="/assets/img/posts/ios-scrollview-2.png" alt="ios-scrollview-2"></p>
<br>
**[3] UIScrollView의 Content Layout Guides 체크를 해제한다.**<br>
<p align="center"><img src="/assets/img/posts/ios-scrollview-3.png" alt="ios-scrollview-3"></p>
<br>
**[4] UIScrollView 안에 UIView를 생성한다.**<br>
- UIView의 상하좌우를 0, 0, 0, 0으로 설정<br>
- UIScrollView를 사용할 때에는 UIScrollView 안의 UIView의 높이에 따라 스크롤 크기가 달라지므로 UIView의 높이를 고정<br>
<p align="center"><img src="/assets/img/posts/ios-scrollview-4.png" alt="ios-scrollview-4"></p>
<br>
**[5] UIView의 Horizontally in Container에 체크한다.**<br>
<p align="center"><img src="/assets/img/posts/ios-scrollview-5.png" alt="ios-scrollview-5"></p>
<br>
**6. 테스트를 위해 UIScrollView 맨 밑에 UIButton을 추가한다.**<br>
<p align="center"><img src="/assets/img/posts/ios-scrollview-6.png" alt="ios-scrollview-6"></p>
<br>
**\# 실행결과**<br>
<p align="center"><img src="/assets/img/posts/ios-scrollview-7.gif" alt="ios-scrollview-7"></p>