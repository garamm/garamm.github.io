---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 10-1. 문자열 검색 / 고지식한 검색"
author: 임가람
date: "2021-09-18 11:46:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 고지식한 검색
### 용어 정리
이 장에서 쓰이는 용어들을 정리합니다.<br>
- 본문(Text): 탐색 대상이 되는 문자열을 의미
- 패턴(Pattern): 검색어를 의미
- 이동(Shift): 본문에서의 탐색 위치를 옮기는 것
<br>

### 고지식한 검색
본문 문자열을 처음부터 끝까지 차례대로 순회하면서 패턴 내의 문자들을 일일이 비교하는 방식<br>
![고지식한 검색 1](/assets/img/posts/2021-09-18-string-search-brute-force-1.png){: width="700" height="150" }

![고지식한 검색 2](/assets/img/posts/2021-09-18-string-search-brute-force-2.png){: width="700" height="150" }

![고지식한 검색 3](/assets/img/posts/2021-09-18-string-search-brute-force-3.png){: width="700" height="150" }

![고지식한 검색 4](/assets/img/posts/2021-09-18-string-search-brute-force-4.png){: width="700" height="150" }

![고지식한 검색 5](/assets/img/posts/2021-09-18-string-search-brute-force-5.png){: width="700" height="150" }


<br>

이러한 알고리즘을 가리켜 고지식한 검색(Naive Search), 또는 무식한 검색(Brute Force Search)라고 한다.<br>
고지식한 검색은 본문의 길이를 $N$, 패턴의 길이를 $M$이라고 했을 때, 본문 속에 존재하는 패턴을 찾기 위해 최악의 경우 $N \times M$ 번의 비교를 수행한다.<br>
<br>

---
## 고지식한 검색의 구현
아래 예제는 본문을 담고 있는 파일과 패턴을 입력받아서, 패턴과 일치하는 본문의 줄 번호와 열 번호를 출력한다.<br>

[본문을 담고 있는 파일 보기](/assets/file/kjv.txt){:target="_blank"}
<br>
BruteForce.h
```c
#ifndef BRUTEFORCE_H
#define BRUTEFORCE_H

int BruteForce(char* Text, int TextSize, int Start, 
               char* Pattern, int PatternSize );

#endif
```
BruteForce.c
```c
#include "BruteForce.h"

int BruteForce(char* Text, int TextSize, int Start, 
               char* Pattern, int PatternSize )
{
    int i = 0;
    int j = 0;

    for ( i=Start; i<=TextSize - PatternSize ; i++ )
    {
        for ( j=0; j<PatternSize; j++ )
        {
            if ( Text[i+j] != Pattern[j] )
                break;
        }

        if ( j >= PatternSize )
            return i;
    }

    return -1;
}
```
Test_BruteForce.c
```c
#include <stdio.h>
#include <string.h>
#include "BruteForce.h"

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
            BruteForce( Text, strlen(Text), 0, Pattern, PatternSize );
        
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
>BruteForce kjv.txt Faithful
line:17178, column:9 : Prv27:6 Faithful are the wounds of a friend; but the kisses of an enemy are deceitful.
line:29648, column:9 : 1Th5:24 Faithful is he that calleth you, who also will do it.
line:31031, column:97 : Rev19:11 And i saw heaven opened, and behold a white horse; and he that sat upon him was called Faithful and True, and in righteousness he doth judge make war.
```




<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>



