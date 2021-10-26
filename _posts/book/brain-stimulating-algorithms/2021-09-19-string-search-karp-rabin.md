---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 10-2. 문자열 검색 / 카프-라빈 알고리즘"
author: 임가람
date: "2021-09-19 21:10:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 카프-라빈 알고리즘
### 해시 값을 활용한 문자열 검색
패턴의 해시 값과 본문 안에 있는 하위 문자열의 해시 값만을 비교한다.<br>
$S$는 문자열, $S[n]$은 문자열 내의 $n$번째 문자, 그리고 $m$을 문자열의 길이라고 할 때 해시 함수는 다음과 같이 정의한다.<br>
$H_i=S[i] \times 2^{(m-1)} + S[i+1] \times 2^{(m-2)} + \ldots + S[i+m-2] \times 2^i + S[i+m-1] \times 2^0$ <br>

위 식을 보면 문자열의 각 요소에 일일이 접근해서 수를 곱한 다음 덧셈 연산까지 하므로 고지식한 검색에서 하는 연산보다 더 많다.<br>
카프와 라빈은 해시 함수의 비용을 줄이는 방법을 아래와 같은 방식으로 찾아냈다.<br>

$
\begin{aligned}
H_0&=S[0] \times 2^3 + S[1] \times 2^2 + S[2] \times 2^1 + S[3] \times 2^0 \\\\\\
H_1&=S[1] \times 2^3 + S[2] \times 2^2 + S[3] \times 2^1 + S[4] \times 2^0 \\\\\\
&= 2(S[1] \times 2^2 + S[2] \times 2^1 + S[3] \times 2^0) + S[4] \times 2^0 \\\\\\
&= 2(S[0] \times 2^3 + S[1] \times 2^2 + S[2] \times 2^1 + S[3] \times 2^0 - S[0] \times 2^3) + S[4] \times 2^0 \\\\\\
&= 2(H_0 - S[0] \times 2^3) + S[4] \times 2^0
\end{aligned}
$


원래의 해시 함수로는 비교할 때마다 매번 패턴의 길이 $m$만큼 문자에 접근해야 하지만, 새 해시 함수를 이용하면 항상 2개의 문자에만 접근하면 된다. 즉 새로운 해시 함수는 다음과 같이 정의할 수 있다.<br>

$
\begin{aligned}
H_i &= 2(H_{i-1} - S[i-1] \times 2^{m-1}) + S[i+m-1] \times 2^0 \\\\\\
&= 2(H_{i-1} - S[i-1] \times 2^{m-1}) + S[i+m-1] 
\end{aligned}
$

이 해시 함수를 사용하면 총 탐색 시간이 패턴의 길이와 상관없이 본문의 길이에만 영향을 받게 되어 탐색 성능이 향상된다. 다만, 새 해시 함수는 $i$가 0일 때, 즉 이전 해시 값이 미리 구해지지 않은 경우에는 사용할 수 없으므로 최초의 해시 값은 기존의 해시 함수를 사용해야 한다.<br>
최초의 해시 함수나 새 해시 함수 모두 문자열의 길이가 늘어나면 해시 값도 같이 커진다는 문제가 있다. 따라서 해시 값을 일정 범위 안에 가둘 필요가 있는데, 이를 위해서 해시 함수의 결과를 어느 값으로 나눈 나머지를 해시 값으로 사용한다. '어느 값'을 $q$라고 할 때 $q$가 들어간 해시 함수는 다음과 같다.<br>

$
H_i =\begin{cases}
S[i] \times 2^{m-1} + S[i+1] \times 2^{m-2} + \cdots + S[i+m-2] \times 2^1 + s[i+m-1] \times 2^0 \; mod \; q,\quad i=0 \\\\\\
2(H_{i-1} - S[i-1] \times 2^{m-1}) + S[i+m-1] \; mod \; q,\quad i>0
\end{cases}
$

<br>

### 해시 값을 활용한 문자열 검색 예제
$q$의 값은 2147483647(int의 최대값), 본문과 패턴은 다음과 같다.
```
본문: ABACCEFABADD
패턴: CCEFA
```
패턴 길이 $m$은 5이다. 먼저 패턴의 해시 값과 본문[0...4]의 해시 값을 구한다. 패턴이나 본문[0...4] 모두 '이전 해시 값'이 없으니 다음의 해시 함수를 이용해서 해시 값을 계산한다.<br>
$(S[i] \times 2^{m-1} + S[i+1] \times 2^{m-2} + \cdots + S[i+m-2] \times 2^1 + S[i+m-1] \times 2^0) \; mod \; q,$

패턴은 2089, 본문[0...4]는 2029가 나온다. 이 둘은 서로 일치하지 않으므로 검색을 계속하기 위해 본문의 위치를 오른쪽으로 한 칸 이동한다.<br>
![카프-라빈 알고리즘 예제 1](/assets/img/posts/2021-09-19-string-search-karp-rabin-1.png){: width="700" height="150" }

앞 단계와는 다르게 본문 [0...4]의 해시 값을 갖고 있다. 그러므로 이 단계 부터는 다음의 해시 함수를 사용할 수 있다.<br>
$(2(H_{i-1} - S[i-1] \times s^{m-1}) + S[i+m-1]) \; mod \; q,$

본문 [1...5]의 해시 값을 본문 [0...4]의 해시 값과 새 해시 함수를 이용하여 계산하면, 결과는 2047로 패턴의 해시 값 2089와 일치하지 않는다. 검색을 계속하기 위해 본문의 위치를 오른쪽으로 한 칸 이동한다.<br>
![카프-라빈 알고리즘 예제 2](/assets/img/posts/2021-09-19-string-search-karp-rabin-2.png){: width="700" height="150" }

본문 [1...5]의 해시 값을 이용해서 본문 [2...6]의 해시 값을 계산한다. 2052는 일치하지 않으므로 본문의 위치를 오른쪽으로 한 칸 이동한다.<br>
![카프-라빈 알고리즘 예제 3](/assets/img/posts/2021-09-19-string-search-karp-rabin-3.png){: width="700" height="150" }

본문 [3...7]의 해시 값은 2089이다. 값이 일치한다.<br>
![카프-라빈 알고리즘 예제 4](/assets/img/posts/2021-09-19-string-search-karp-rabin-4.png){: width="700" height="150" }

카프-라빈 알고리즘이 해시 함수에 기반하고 있기 때문에 서로 다른 문자열에서 얻어낸 해시 값이 동일한 경우(해시 함수의 충돌)가 발생할 수 있다.<br>
그렇기 때문에 패턴의 해시 값과 일치하는 부분을 찾기는 했지만, 찾아낸 문자열이 실제로 패턴과 일치하는지도 검증을 해야한다.<br>
<br>

---
## 카프-라빈 알고리즘의 구현
[본문을 담고 있는 파일 보기](/assets/file/kjv.txt){:target="_blank"}

<br>

KarpRabin.h
```c
#ifndef KARPRABIN_H
#define KARPRABIN_H

int KarpRabin( char* Text, int TextSize, int Start, 
               char* Pattern, int PatternSize );

int Hash( char* String, int Size );
int ReHash( char* String, int Start, int Size, int HashPrev, int Coefficient);
#endif
```
KarpRabin.c
```c
#include "KarpRabin.h"
#include <stdio.h>
#include <math.h>

int KarpRabin(char* Text, int TextSize, int Start, char* Pattern, int PatternSize )
{
    int i = 0;
    int j = 0;
    int Coefficient = pow( 2, PatternSize - 1 );

    int HashText    = Hash( Text,    PatternSize );
    int HashPattern = Hash( Pattern, PatternSize );

    for ( i=Start; i<=TextSize - PatternSize; i++ )
    {
        HashText = ReHash( Text, i, PatternSize - 1, HashText, Coefficient);
        
        if ( HashPattern == HashText )
        {
            for ( j=0; j<PatternSize; j++ )
            {
                if ( Text[i+j] != Pattern[j] )
                    break;
            }

            if ( j >= PatternSize )
                return i;
        }
    }

    return -1;
}

/* 패턴과 본문[0...m-1]의 해시 값을 구함 */
int Hash( char* String, int Size )
{
    int i = 0;
    int HashValue = 0;

    for ( i=0; i<Size; i++ )
    {
        HashValue = String[i] + ( HashValue * 2 );
    }
    /*
        카프-라빈 알고리즘에 따르면 나머지 연산을 해야하는데 소스코드에서는 나머지 연산을 하지 않는다.
        int의 최대 값을 q로 정해서 해시 값이 q보다 더 커지면 오버플로(Overflow)된 값을 그대로 사용하면 되기 때문이다.
        ReHash() 함수도 동일한 방식을 사용한다.
    */
    return HashValue;
}

/* 본문[i...(i+m-1)]의 해시 값을 구함 */
int ReHash( char* String, int Start, int Size, int HashPrev, int Coefficient )
{   /* 거듭제곱은 비용이 많이 소요되는 연산이고 한 패턴에 대해 2^(m-1)은 항상 같은 값을 가지므로 밖에서 계산한 후 매개변수 Coefficient로 넘겨 받아서 사용하는 것이 경제적이다.*/
    if ( Start == 0 )
        return HashPrev;
    
    return String[ Start + Size - 1] + 
           ( (HashPrev - Coefficient * String [Start-1] ) * 2 );
}
```
Test_KarpRabin.c
```c
#include <stdio.h>
#include <string.h>
#include "KarpRabin.h"
#include <math.h>

#define MAX_BUFFER 512

int main(int argc, char** argv)
{
    char  Text[MAX_BUFFER] = "My name is pattern";
    char* Pattern = "pattern";
    int   PatternSize = 4;
    int h0, h1, rh1;

    int result = KarpRabin( Text, strlen(Text), 0, Pattern, strlen(Pattern));
    printf("%d\n", result);

    h0 = Hash(Pattern, PatternSize);
    h1 = Hash(Pattern+1, PatternSize);
    rh1 = ReHash(Pattern, 1, PatternSize, h0, (int)pow(2, PatternSize-1));

    printf("h0:%d\n", h0);
    printf("h1:%d\n", h1);
    printf("rh1:%d\n", rh1);

/* 주석의 예제는 파일 경로를 입력 받아 문자열을 찾는 예제이다. */
/*   char* FilePath;
    FILE* fp;

    char  Text[MAX_BUFFER];
    char* Pattern;
    int   PatternSize = 0;
    int   Line        = 0;

    if ( argc < 3 )
    {
        printf("Usage: %s <FilePath> <Pattern>\n", argv[0] );
        return 1;
    }

    FilePath = argv[1];
    Pattern  = argv[2];

    PatternSize = strlen( Pattern );

    if ( (fp = fopen( FilePath, "r"))  == NULL )
    {
        printf("Cannot open file:%s\n", FilePath);
        return 1;
    } 

    while ( fgets( Text, MAX_BUFFER, fp ) != NULL )
    {
        int Position = 
            KarpRabin( Text, strlen(Text), 0, Pattern, PatternSize );
        
        Line++;

        if ( Position >= 0 ) 
        {
            printf("line:%d, column:%d : %s", Line, Position+1, Text);
        }
    }

    fclose( fp );*/

    return 0;
}
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>