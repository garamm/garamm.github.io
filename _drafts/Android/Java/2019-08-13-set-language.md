---
layout: posts
title: "[Android/Java] 다국어 설정"
comments: true
categories: [Android/Java]
---

1) 프로젝트폴더/app/src/main/res 폴더 안에 다음과 같이 values-국가코드 형식으로 폴더를 생성
![img1](/img/2019-08-13-set-language-1.png)

2) 각각의 values 폴더 안에 strings.xml 파일을 생성

3) 각 폴더별 국가코드에 따라 언어를 작성
![img2](/img/2019-08-13-set-language-2.png)
```
default : <string name="hello_world">Hello world!</string>
en : <string name="hello_world">Hello world!</string>
es : <string name="hello_world">¡Hola!</string>
ja : <string name="hello_world">こんにちは!</string>
ko : <string name="hello_world">안녕!</string>
zh : <string name="hello_world">Nǐ hǎo!</string>
```

4) 디바이스의 언어 상태에 따라 앱 내의 언어가 달라짐