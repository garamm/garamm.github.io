---
layout: post
title: "[Go] Go에서 한글 2Byte로 계산하기"
author: 임가람
date: "2021-03-12 22:47:00"
categories: [Go]
tags: [encoding]
---

### Intro

개발을 하다 보면 소켓 통신을 할 때 한글을 전송해야 할 때가 있는데요<br>
그중에서도 20 Byte 길이로 보내되 나머지를 공백으로 채워서 보내라고 하는 경우가 있습니다.<br>
여기서 문제가 발생합니다.<br>
한글은 UTF-8에서 3 Byte로 계산하고, EUC-KR에서는 2 Byte로 계산합니다. 그래서 EUC-KR을 기준으로 할 땐 계산한 자릿수가 맞지 않아 오류가 발생합니다.<br>
그렇기 때문에 이번 글에서는 한글을 2 Byte로 계산해주는 패키지를 소개합니다.<br>


### 패키지 설치
```console
go get github.com/suapapa/go_hangul
```

### 코드 작성
예제)<br>
20 byte 길이의 배열이 있다.<br>
한글을 EUC-KR(2byte)로 계산하여 byte 배열에 텍스트를 넣은 후<br>
나머지를 공백으로 채운다.<br>
\* 값 출력시 길이를 확인하기 위해 "]"를 추가로 출력했다.
```go
package main

import (
	"fmt"
	"golang.org/x/text/encoding/korean"
	"golang.org/x/text/transform"
)

func main() {
	src := "임가람"
	got, _, _ := transform.String(korean.EUCKR.NewEncoder(), src)
	// fmt.Println([]byte(got), len(got))
	fmt.Println(fillSpanceInByteArr(src, len(got), 20))
}

func fillSpanceInByteArr(str string, strLen int, byteLen int) string {
	space := ""
	for i := 0; i < byteLen-strLen; i++ {
		space = space + " "
    }
	return str + space + "]"
}
```

\# 참고<br>
[고 언어에서 로컬 인코딩 cp949 처리하기: HOMIN LEE'S BLOG](http://suapapa.github.io/blog//post/handling_cp949_in_go/){:target="_blank"}