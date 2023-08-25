---
layout: post
title: "[VSCode] 자동완성 단축키 만들기"
categories: [etc]
tags: ["vscode", "snippets"]
date: "2023-07-09 17:25"
---

[1\] File > Preferences > Configure User Snippets<br>
![vscode-custom-snippets-1](/assets/img/posts/vscode-custom-snippets-1.png){:width="600"}<br>
<br>
[2\] 언어 선택<br>
![vscode-custom-snippets-2](/assets/img/posts/vscode-custom-snippets-2.png){:width="600"}<br>
<br>
[3\] 자동 완성 정보 입력
```javascript
// 예시
// $1, $2...는 커서가 멈출 위치를 의미
{
	"Print to console": {
		"prefix": "log",
		"body": [
			"console.log($1);"
		],
		"description": "Log output to console"
	}
}
```
<br>
[주니어 개발자의 성장기: [VSCode] 자동완성 단축키 추가](https://pygmalion0220.tistory.com/entry/VsCdoe-자동완성-단축키-추가){:target="_blank"}