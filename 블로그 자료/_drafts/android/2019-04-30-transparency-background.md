---
layout: post
title: "[Android/Java] 레이아웃 투명 배경"
category: android
date: "2019-04-30"
---

1. xml에서 투명 배경 적용하는 방법
android:background="@null"
또는
android:background="@android:color/transparent"
또는
android:background="#00000000"


2. java에서 투명 배경 적용하는 방법
viewName.setBackgroundResource(0);