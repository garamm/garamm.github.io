---
layout: singlePage
title: "Go 시작하기"
date: "2020-06-24"
---

### 1\. Hello World
```go
package main

import (
    "fmt"
)

func main() {
    fmt.Println("Hello World")
}
```

<br>

### 2\. 변수 선언
```go
// const : 값이 변하지 않음
// var : 값이 변할 수 있음
func main() {
    // 방법 1. var 변수명 = 값
    // 자료형을 쓰지 않을 땐 항상 값을 함께 저장해야 함
    const a = "a"

    // 방법 2. var 변수명 자료형
    // int, string, bool 등등
    var b string
    b = "b"
        
    // 방법 3. 변수명 := 값
    c := "c"
}
```

<br>

### 3\. 문자열 사용하기

[1] 문자열 기본
```go
func main() {
    // 문자열 변수 선언
    var s1 = "안녕하세요"
	
    // 여러줄로 된 문자열 변수 선언
    var s2 string=`안녕하세요
Hello World`

    fmt.Println(s1)
    fmt.Println(s2)
}
/* 출력결과
안녕하세요
안녕하세요
Hello World
*/
```

[2] 문자열의 길이
```go
package main
 
import (
	"fmt"
    "unicode/utf8"
)
     
func main(){
    // 영어 문자열 길이 구하기
    var s1 string="English"
    fmt.Println(len(s1))
	
    // 한글, 중국어, 일본어 등의 문자열 길이 구하기
    // 한글을 유니코드로 저장하면 0xed, 0x95, 0x9c, 0xea, 0xb8, 0x80이기 때문에 len()을 사용하면 6이 출력됨
    // 그러므로 "unicode/utf8" 패키지의 함수를 사용하여 문자열의 길이를 구함
    var s2 string="한글"
    fmt.Println(utf8.RuneCountInString(s2))
}
```

[3] 문자열의 연산
```go
func main(){
    s1="Hello"
    var s2="Hello"
    var s3="World"

    fmt.Println(s1==s2) // 두 문자열이 같은지 비교, true
    fmt.Println(s1+s3)  // 문자열 더하기, HelloWorld
}
```