---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 10-3. 문자열 검색 / KMP 알고리즘"
author: 임가람
date: "2021-09-20 21:55:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## KMP 알고리즘
KMP(Knuth-Morris-Pratt) 알고리즘은 '고지식한 알고리즘'처럼 검색 위치를 본문의 왼쪽부터 시작해서 오른쪽으로 옮겨가며 문자를 직접 비교하는 방식으로 동작한다.<br>
그러나 비교할 필요가 없는 부분은 지나치고, 비교한 필요한 부분만 비교를 수행하는 알고리즘이다.<br>
<br>

---
## 접두부, 접미부, 그리고 경계
어느 문자열이든 접두부(Prefix)와 접미부(Suffix)를 갖는다. 다음과 같은 문자열이 있다고 했을 때,
```
BAABABAA
```
이 문자열로부터 얻을 수 있는 접두부와 접미부는 다음과 같다. 이때 빈 문자열도 접두부와 접미부가 될 수 있다.
|접두부|접미부|
|---|---|
|&nbsp;|&nbsp;|
|B|A|
|BA|AA|
|BAA|BAA|
|BAAB|ABAA|
|BAABA|BABAA|
|BAABAB|ABABAA|
|BAABABA|AABABAA|

KMP 알고리즘에서는 빈 문자열이나 BAA처럼 문자열에서 일치하는 접두부와 접미부 쌍을 가리켜 경계(Border)라고 한다.<br>
![KMP 알고리즘 경계](/assets/img/posts/2021-09-20-string-search-kmp-1.png){: width="700" height="150" }

KMP 알고리즘은 이 경계를 활용해서 불필요한 문자 비교를 피한다. 예를 들어 아래와 같은 본문과 패턴으로 검색을 한다고 하자.<br>
```
본문: BAABAABAB
패턴: BAABAB
```
검색을 시작해보면 본문과 패턴 0~4번 문자열(BAABA)까지는 서로 일치한다. 그런데 5번 문자에서 불일치가 일어난다.<br>
![KMP 알고리즘 예제 1](/assets/img/posts/2021-09-20-string-search-kmp-2.png){: width="700" height="150" }

불일치가 일어나기 전까지 본문과 일치했던 0~4번의 문자열을 보면 BAABA는 빈 문자열과 BA, 두 개의 경계를 갖고 있다.<br>
![KMP 알고리즘 예제 2](/assets/img/posts/2021-09-20-string-search-kmp-3.png){: width="700" height="150" }

본문[0...4]와 패턴[0...4]는 서로 일치한다. 즉 두 문자열의 경계가 같다. 이는 본문[0...4]의 접미부는 패턴[0...4]의 접두부와 동일하다. 따라서 패턴 전체를 오른쪽으로 쭉 밀어서 패턴의 접두어와 본문[0...4]의 접미부를 일치하게 만들어 놓으면 곧바로 5번부터 비교를 재개할 수 있다. 이때 패턴의 검색 위치 이동 거리는 일치하는 부분 문자열(BAABA)의 길이(5)에서 경계(BA)의 길이(2)를 뺀 것과 같다.<br>
![KMP 알고리즘 예제 3](/assets/img/posts/2021-09-20-string-search-kmp-4.png){: width="700" height="150" }

따라서 KMP 알고리즘은 본문의 길이가 n일 떄, 최대 n번만큼만 비교를 수행하면 본문 내에서 패턴과 일치하는 문자열의 위치를 알아낼 수 있다.<br>
<br>

---
## 경계 정보를 미리 계산하기
KMP 알고리즘에서 검색을 수행하는 중간에 매번 경계를 찾는다면 그 비용도 계속 늘어날 것이다. 그래서 KMP 알고리즘은 검색을 수행하기 전에 미리 패턴으로부터 경계의 정보를 갖는 테이블을 만든다.<br>
예를 들어 `BAABABAA` 라는 패턴이 있고, 이 패턴으로 검색을 시도했을 때 첫 번째 문자부터 불일치가 일어난다고 하자. KMP 알고리즘에서는 불일치가 일어난 위치 이전의 일치 접두부에서 최대 경계를 갖고, 이 경계의 너비를 이용해서 검색 위치를 뛰어넘는다. 그런데 첫 번째 문자는 일치 접두부가 존재하지 않는다. 그래서 첫 번째 문자의 경계의 너비는 항상 -1이다. (0은 이전 접두부가 존재하지만 경계가 없을 때에 경계의 너비로 사용한다.)<br>
![KMP 알고리즘 경계 테이블 1](/assets/img/posts/2021-09-20-string-search-kmp-5.png){: width="700" height="150" }

이번엔 2번째 문자에서 불일치가 일어났다고 가정했을 때, 2번째 문자의 일치 접두부는 B 하나이다. 따라서 경계가 존재하지 않는다. 이전 접두부의 최대 경계 너비는 0이다.<br>
3번째 문자 또한 경계가 존재하지 않으므로 0이다. 4번쨰 문자도 마찬가지이다.<br>
![KMP 알고리즘 경계 테이블 2](/assets/img/posts/2021-09-20-string-search-kmp-6.png){: width="700" height="150" }

5번째 문자에서 불일치가 일어나는 경우, 일치 접두부는 BAAB이므로 최대 경계가 B로써 너비는 1이다.<br>
6번째 문자의 일치 접두부 BAABA는 최대 경계가 BA로 너비가 2이다.<br>
이런 방식으로 테이블을 채워 나가면 다음과 같다.<br>
![KMP 알고리즘 경계 테이블 3](/assets/img/posts/2021-09-20-string-search-kmp-7.png){: width="700" height="150" }

완성된 테이블을 보면 경계 테이블의 크기가 패턴의 크기보다 1만큼 더 큼을 알 수 있다.<br>
테이블의 마지막 칸은 본문 내의 문자열이 패턴과 일치하지만 이어서 탐색을 계속하고자 할 때 사용한다. 예를 들어 패턴 전체와 일치하는 부분을 찾았는데, 그 부분 뒤에 이어지는 본문의 문자가 패턴의 첫번째 문자와 일치하지 않는 경우에 이 경계의 너비 값을 사용한다.<br>
<br>
마지막 칸을 채워보면 일치 접두부는 BAABABAA이고 최대 경계는 BAA로 길이는 3이다. 이 값을 기입하면 테이블이 완성된다.<br>
![KMP 알고리즘 경계 테이블 4](/assets/img/posts/2021-09-20-string-search-kmp-8.png){: width="700" height="150" }

<br>

불일치가 발생했을 때 검색 위치의 이동 거리는 다음과 같다.<br>
```
이동 거리 = 일치 접두부의 길이 + 최대 경계 너비
```
예를 들어 본문과 패턴이 0~4번 문자까지는 일치하고, 5번 문자에서 불일치가 발생했다면, 패턴의 검색 위치는 5-2=3 이다.<br>
![KMP 알고리즘 경계 불일치](/assets/img/posts/2021-09-20-string-search-kmp-9.png){: width="700" height="150" }

<br>

---
## KMP 알고리즘의 구현
[본문을 담고 있는 파일 다운받기](경로)
<br>

KnuthMorrisPratt.h
```c
#ifndef KNUTHMORRISPRAT_H
#define KNUTHMORRISPRAT_H

#include <stdio.h>

int  KnuthMorrisPratt(char* Text, int TextSize, int Start, 
                      char* Pattern, int PatternSize );

void Preprocess( char* Pattern, int PatternSize, int* Border );

#endif
```
KnuthMorrisPratt.c
```c
#include "KnuthMorrisPratt.h"
#include <stdlib.h>

/* 최대 경계의 너비를 보관하는 테이블을 만든다. */
void Preprocess( char* Pattern, int PatternSize, int* Border ) 
{
    int i = 0;
    int j = -1;

    Border[0] = -1;

    while ( i < PatternSize )
    {
        while ( j > -1 && Pattern[i] != Pattern[j] )
            j = Border[j];

        i++; 
        j++;

        Border[i] = j;
    }
}

/* 검색을 담당한다. */
int  KnuthMorrisPratt(char* Text, int TextSize, int Start, 
                      char* Pattern, int PatternSize )
{
    int i = Start;
    int j = 0;
    int Position = -1;
    
    int* Border = (int*)calloc( PatternSize + 1, sizeof( int ) );

    Preprocess( Pattern, PatternSize, Border );

    while ( i < TextSize )
    {
        while ( j >= 0 && Text[i] != Pattern[j] )
            j = Border[j];

        i++;
        j++;

        if ( j == PatternSize )
        {
            Position = i - j;
            break;
        }
    }

    free( Border );
    return Position;
}
```
Test_KnuthMorrisPratt.c
```c
#include <stdio.h>
#include <string.h>
#include <time.h>

#include "KnuthMorrisPratt.h"

#define MAX_BUFFER 512

int main(int argc, char** argv)
{
    char* FilePath;
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
            KnuthMorrisPratt( Text, strlen(Text), 0, Pattern, PatternSize );
        
        Line++;

        if ( Position >= 0 ) 
        {
            printf("line:%d, column:%d : %s", Line, Position+1, Text);
        }
    }

    fclose( fp );

    return 0;
}
```
실행결과
```
>KnuthMorrisPratt kjv.txt "The Prince of Peace"
line:17838, column:200 : Isa9:6 For unto us a child is born, unto us a son is given: and the government shell be upon his shoulder: and his name shall be called Wonderful, Counsellor, The mighty God, The everlasting Father, The Prince of Peace.
```
<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>



