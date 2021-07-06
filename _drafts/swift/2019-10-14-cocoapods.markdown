---
layout: post
title: "[iOS] CocoaPods 사용하기"
category: swift
date: "2019-10-14"
---

1. CocoaPods 설치
$ sudo gem install cocoapods

2. 프로젝트 경로로 이동
$ cd MyProject

3. 프로젝트 안에 Podfile 생성
$ pod init

4. Podfile 수정
![img1](/img/2019-10-14-cocoapods-1.png)

5. Podfile을 수정 및 저장한 뒤 라이브러리 설치
$ pod install

6. 라이브러리 적용
 ㄴ 프로젝트 파일 명 선택
 ㄴ Frameworks, Libraries, and Embedded Content 항목의 + 버튼 클릭
 ㄴ 추가한 라이브러리 선택 및 Add
![img2](/img/2019-10-14-cocoapods-2.png)

참고) 프로젝트 파일 열 때 .xcodeproj가 아닌 .xcworkspace를 열어야 함


