---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 10-4. 문자열 검색 / 보이어-무어 알고리즘"
author: 임가람
date: "2021-09-21 22:39:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 보이어-무어(Boyer-Moor) 알고리즘
보이어-무어 알고리즘은 특이하게 이동은 왼쪽에서 오른쪽으로 하지만, 문자열을 오른쪽에서 왼쪽으로 비교해 나간다.<br>
우리가 사용하는 대부분의 프로그램이 이 알고리즘을 사용한다.<br>
보이어-무어 알고리즘은 패턴의 오른쪽 끝에 있는 문자가 불일치하고, 이 문자가 패턴 내에 존재하지 않는 경우, 이동 거리는 무려 패턴의 길이만큼이 된다.<br>
아래의 예에서 오른쪽 끝 문자가 패턴 안에서 발견되지 않으면 패턴의 길이만큼 검색 위치를 이동시켜 비교를 재개한다.<br>
![보이어-무어 알고리즘](/assets/img/posts/2021-09-21-string-search-boyer-moore-1.png){: width="700" height="150" }

보이어-무어 알고리즘엔 두 가지 종류의 이동이 있다.<br>
- 나쁜 문자 이동(Bad Character Shift)
- 착한 접미부 이동(Good Suffix Shift)
이 두가지 이동 방법 모두 불일치가 발생하는 경우에 최대의 효율로 이동하는 방법을 정의한다. 본문과 패턴을 비교할 때 불일치가 발생하면 나쁜 문자 이동과 착한 접미부 이동 양쪽을 검토해서 더 멀리 옮겨가는 이동을 수행한다.<br>
<br>

---
## 나쁜 문자 이동
나쁜 문자 이동은 본문과 패턴 사이를 불일치시키는 '본문의 문자'를 이동시킨다는 뜻이다.<br>
나쁜 문자 이동은 다음 두 단계를 수행한다.<br>
1. 패턴에서 나쁜 문자를 찾는다.
2. 찾아낸 패턴의 나쁜 문자의 위치가 본문의 나쁜 문자 위치와 일치하게 패턴을 이동시킨다.

나쁜 문자와 같은 문자가 패턴 내에서 두 개 이상 나오는 경우, 발견된 문자 중 가장 오른쪽에 있는 것을 본문의 나쁜 문자에 맞춘다. 아래 그림을 보면 본문의 3번 문자는 B, 패턴의 3번 문자는 C이기 때문에 불일치한다. 여기서 나쁜 문자는 B이므로 이것을 패턴에서 찾는다. B가 패턴에서 0, 1번에서 각각 발견되므로 발견된 문자 중 가장 오른쪽에 있는 문자는 1 이므로 패턴의 1번에 있는 B를 본문의 나쁜 문자의 위치에 일치하도록 패턴을 이동시킨다.<br>
![나쁜문자이동 1](/assets/img/posts/2021-09-21-string-search-boyer-moore-2.png){: width="700" height="150" }

나쁜 문자 이동은 실패하는 경우도 있다. 아래 그림을 보면 나쁜 문자는 A인데 패턴에서 나쁜 문자를 찾으면 0, 2에서 나타난다. 그런데 2가 가장 오른쪽에 있으므로 이것을 본문의 나쁜 문자에 맞추기 위해 패턴을 이동하면 패턴은 오른쪽이 아니라 왼쪽(마이너스 방향)으로 간다.
![나쁜문자이동 2](/assets/img/posts/2021-09-21-string-search-boyer-moore-3.png){: width="700" height="150" }

이런 경우에는 '착한 접미부 이동'을 사용한다.<br>

---
## 착한 접미부 이동
보이어-무어 알고리즘은 패턴을 오른쪽에서부터 비교하기 때문에 패턴에는 본문과 일치하는 접미부가 나타나게 된다. 이렇게 일치하는 접미부를 '착한 접미부'라고 부른다.<br>
착한 접미부 이동을 하는 상황은 두 가지로 나뉜다.<br>

### 착한 접미부 이동을 하는 상황 1
불일치가 일어났을 때 착한 접미부와 동일한 문자열이 패턴의 착한 접미부 왼쪽에 존재하는 경우<br>
아래에 있는 그림을 보면 2번 문자에서 본문의 A와 패턴 B가 불일치한다. 그래서 착한 접미부 AB를 패턴에서 찾고 이 위치를 본문의 착한 접미부의 위치에 일치하도록 이동시킨다.<br>
![착한접미부이동 1](/assets/img/posts/2021-09-21-string-search-boyer-moore-4.png){: width="700" height="150" }

그런데 일치하는 부분이 없으면 두 번째 경우를 확인하고, 두 번째 경우에도 해당되지 않으면 패턴의 길이만큼 검색 위치를 오른쪽으로 이동시키면 된다.
<br>

### 착한 접미부 이동을 하는 상황 2
착한 접미부가 패턴 안에 존재하지는 않지만, 착한 접미부의 접미부가 패턴의 접두부와 일치하는 경우<br>
이 경우에는 착한 접미부 전체가 아닌 '착한 접미부의 접미부'와 일치하는 패턴의 접두부를 이동시키면 된다.<br>
다음 그림을 보면 착한 접미부가 AAB인데, 패턴에는 일치하는 문자열이 없다.<br>
![착한접미부이동 2](/assets/img/posts/2021-09-21-string-search-boyer-moore-5.png){: width="700" height="150" }

그런데 착한 접미부의 접미부 AB는 패턴의 접두부와 일치한다. 패턴의 접두부가 착한 접미부의 접미부에 일치하도록 다음과 같이 이동시킨다.<br>
![착한접미부이동 3](/assets/img/posts/2021-09-21-string-search-boyer-moore-6.png){: width="700" height="150" }

<br>

---
## 보이어-무어 알고리즘의 전처리 과정
나쁜 문자 이동과 착한 접미부 이동을 하려면 사전에 가공된 정보가 필요하기 때문에 그를 위한 테이블을 만들어줘야 한다.<br>

### 나쁜 문자 이동 테이블 만들기
먼저 테이블을 하나 준비하고, 패턴을 왼쪽에서 오른쪽으로 읽어나가면서 패턴에 있는 각 문자의 위치를 이 테이블에 기록한다. 이렇게 하면 패턴 안에 중복되는 문자들이 있어도 가장 오른쪽에 있는 문자의 위치만 테이블에 남게 된다.<br>
아래의 그림을 보면 ABAAB 패턴에서 A는 0, 2, 3번에 위치하지만 테이블에는 가장 오른쪽에 있는 3을 입력하고 B 또한 1, 4번에 위치하지만 가장 오른쪽인 4를 테이블에 입력한다.(65, 66, 67, 68은 ASCII 코드로 'A', 'B', 'C', 'D'이다.). 패턴 내에 존재하지 않는 문자들은 모두 -1로 입력한다.<br>
![나쁜 문자 이동 테이블 만들기](/assets/img/posts/2021-09-21-string-search-boyer-moore-7.png){: width="700" height="150" }

테이블에 입력된 위치는 곧 불일치가 일어났을 때의 이동 거리이다.
<br>

### 착한 접미부 이동 테이블 만들기
착한 접미부 이동에는 두 가지 경우가 있기 때문에 테이블 역시 두 가지 경우를 고려해서 만들어야 한다.<br>
착한 접미부의 존재 여부를 검색하기 전에 확인하고 존재한다면 이동 거리를 테이블 않에 넣어두고, 불일치가 일어났을 때 착한 접미부를 확인하자마자 바로 이동거리를 얻을 수 있도록 미리 테이블을 만든다.<br>
착한 접미부 이동의 첫 번째 경우는 아래 그림과 같이 불일치가 일어났을 때 착한 접미부가 패턴 안에 존재하는 경우이다.<br>
![착한 접미부 이동 테이블 만들기 1](/assets/img/posts/2021-09-21-string-search-boyer-moore-8.png){: width="700" height="150" }

첫 번째 경우에 착한 접미부 이동 테이블을 만드는 과정은 다음과 같다.<br>
우선 패턴의 길이+1 크기의 테이블을 준비한다. 그 다음에 패턴을 오른쪽에서부터 왼쪽으로 읽으면서 나타나는 접미부 X에 대해 이 접미부를 경계로 가지는 패턴 내의 가장 큰 접미부 Y를 찾는다. 가장 큰 접미부를 찾았으면 `X의 시작 위치 - Y의 시작 위치`를 이동 거리로 입력한다.<br>
그런데 만약 접미부 X가 다른 접미부의 경계가 되지 않거나 경계라 하더라도 이 경계가 왼쪽 방향으로 더 큰 경계로 확장될 수 있다면 이동 거리를 0으로 비워둬야 한다. 이렇게 비워둔 이동 거리는 두 번째 경우를 처리할 때 채우게 된다.<br>
예를 들어 첫 번째 경우에 대해 AABABA라는 패턴이 있고, 이 패턴으로부터 착한 접미부 이동 테이블을 만들어보면, 빈 접미부는 경계가 없으므로 이동 거리는 패턴의 길이(6)+1=7이 된다. 그리고 착한 접미부가 빈 문자열이라는 것은, 일치하는 부분이 전혀 없다는 뜻이므로 이동 거리는 1이 된다.<br>
![착한 접미부 이동 테이블 만들기 2](/assets/img/posts/2021-09-21-string-search-boyer-moore-9.png){: width="700" height="150" }

문자열의 5번에 있는 A를 보면 A를 경계로 가지는 다른 접미부는 ABA, ABABA, AABABA 세 가지이다. 패턴의 시작 부분을 포함하고 있는 경우는 두 번째 경우에 해당되기 때문에 AABABA는 선택 대상에서 제외된다. 남은 것은 ABA와 ABABA인데, ABABA가 더 크므로 ABABA를 선택한다. ABABA의 시작 위치는 1, A의 시작 위치는 5이므로 이동 거리는 5-1=4 이다. 이동 거리 테이블의 5번에 4를 입력한다.<br>
![착한 접미부 이동 테이블 만들기 3](/assets/img/posts/2021-09-21-string-search-boyer-moore-10.png){: width="700" height="150" }

이제 한 칸 더 왼쪽으로 이동해서 4에서 시작하는 접미부 BA를 확인한다. BA를 경계로 갖는 다른 접미부는 BABA이다. 그런데 BABA에서 왼쪽으로 한 칸 더 이동하면 ABABA가 나오고, ABABA는 BA를 왼쪽으로 한 칸 확장한 ABA가 경계이기 때문에 BABA는 왼쪽 방향으로 확장이 가능하다. 따라서 BA의 이동 거리는 0으로 입력해둔다.<br>
한 칸 더 이동해보면 ABA인데, ABA를 경계로 갖는 최대의 접미부는 ABABA이다. ABABA와 ABA는 왼쪽으로 확장할 수도 없기 때문에 이동에 활용할 수 있다. 현재 위치 3에서 ABABA의 시작 위치를 뺀 값 2를 테이블에 입력한다.<br>
나머지 접미부들(BABA, ABABA, AABABA)은 길이가 원래 문자열 길이의 1/2보다 크기 때문에 경계가 될 수 없다. 이들의 이동 거리는 모두 0으로 채운다.<br>
![착한 접미부 이동 테이블 만들기 4](/assets/img/posts/2021-09-21-string-search-boyer-moore-11.png){: width="700" height="150" }

이제 두 번째 경우를 처리한다. 두 번째 경우는 착한 접미부의 접미부와 패턴의 접두부 사이의 거리가 이동 거리가 된다. 즉, 패턴의 양쪽 경계의 거리가 곧 이동 거리가 된다. 첫 번째 경우에 대한 이동 거리는 이미 처리를 했으므로 테이블 내에서 이동 거리가 0인 항목에 대해서만 처리를 하면 된다.<br>
패턴의 왼쪽부터 읽으면서 경계를 이룰 수 있는 가장 너비가 큰 접두부를 찾아보면 접두부 A는 접미부 A와 경계를 이루므로 이동 거리는 5가 된다.<br>
![착한 접미부 이동 테이블 만들기 5](/assets/img/posts/2021-09-21-string-search-boyer-moore-12.png){: width="700" height="150" }

그런데 접두부 AA는 경계를 이룰 수 있는 접미부가 패턴에 존재하지 않는다. 대신 접두부 AA의 접두부 중에 접미부와 경계를 이룰 수 있는 '접두부의 접두부' A를 전택하고, A끼리의 거리 5를 이동 거리로 입력한다.<br>
이동 거리가 0으로 남아 있는 AAB, AABAB 모두 A 외에는 경계를 이룰 수 있는 접두부를 갖고 있지 않으므로 테이블\[2], 테이블\[4]의 이동 거리는 모두 5가 된다.<br>
![착한 접미부 이동 테이블 만들기 6](/assets/img/posts/2021-09-21-string-search-boyer-moore-13.png){: width="700" height="150" }

<br>

---
## 보이어-무어 알고리즘의 구현
[본문을 담고 있는 파일 보기](/assets/file/kjv.txt){:target="_blank"}
<br>

BoyerMoore.h
```c
#ifndef BOYERMOORE_H
#define BOYERMOORE_H

#include <stdio.h>

int  BoyerMoore(char* Text, int TextSize, int Start, 
                char* Pattern, int PatternSize );

void BuildGST( char* Pattern, int PatternSize, int* Suffix, int* GST );
void BuildBCT( char* Pattern, int PatternSize, int* BST );

int  Max( int A, int B);

#endif
```
BoyerMoore.c
```c
#include "BoyerMoore.h"
#include <stdlib.h>

int  BoyerMoore(char* Text, int TextSize, int Start, 
                char* Pattern, int PatternSize )
{
    int BCT[128];
    int* Suffix = (int*)calloc( PatternSize + 1, sizeof( int ) );
    int* GST  = (int*)calloc( PatternSize + 1, sizeof( int ) );
    int i = Start;
    int j = 0;

    int Position = -1;

    BuildBCT( Pattern, PatternSize, BCT );
    BuildGST( Pattern, PatternSize, Suffix, GST );
    
    while (i <= TextSize - PatternSize)
    {
        j = PatternSize - 1;

        while ( j >= 0 && Pattern[j] == Text[i+j] ) 
            j--;

        if (j<0)
        {
            Position = i;
            break;
        }
        else 
        {
            i+= Max( GST[j+1], j-BCT[ Text[i+j] ])  ;
        }
    }

    free ( Suffix );
    free ( GST  );

    return Position;
}

/* 나쁜 문자 이동 */
void BuildBCT( char* Pattern, int PatternSize, int* BCT )
{
    int i;
    int j;

    for ( i=0; i<128; i++ ) 
        BCT[i]=-1;

    for ( j=0; j<PatternSize; j++ )
        BCT[ Pattern[j] ]=j;
}

/* 착한 접미부 이동 */
void BuildGST( char* Pattern, int PatternSize, int* Suffix, int* GST )
{
    /*  Case 1 */
    int i = PatternSize;
    int j = PatternSize + 1;

    Suffix[i]=j; 
    
    while (i>0)
    {
        while (j<=PatternSize && Pattern[i-1] != Pattern[j-1])
        {
            if ( GST[j] == 0 ) 
                GST[j]=j-i;

            j=Suffix[j];
        }

        i--; 
        j--;
        
        Suffix[i] = j;
    }

    /*  Case 2 */
    j = Suffix[0];

    for ( i = 0; i <= PatternSize; i++ )
    {
        if ( GST[i] == 0 ) 
            GST[i] = j;

        if ( i == j ) 
            j = Suffix[j];
    }
}

/* 나쁜 문자 이동 거리와 착한 접두부 이동 거리 중 더 멀리 옮겨나가는 것을 return */
int  Max( int A, int B )
{
    if ( A > B )
        return A;
    else
        return B;
}
```
Test_BoyerMoore.c
```c
#include <stdio.h>
#include <string.h>

#include "BoyerMoore.h"

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
            BoyerMoore( Text, strlen(Text), 0, Pattern, PatternSize );
        
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
>BoyerMoore kjv.txt "Lord of lords"
line:5205, column:52 : Deu10:17 For the LORD your God is God of gods, and Lord of lords, a great God, a mighty, and a terrible, which regardeth not persons, nor taketh reward:
line:16202, column:31 : Psa136:3 O give thanks to the Lord of lords: for his mercy endureth for ever.
line:29806, column:106 : 1Tim6:15 Which in his times he shall shew, who is the blessed and only Potentate, the King of kings, and Lord of lords;
line:30992, column:90 : Rev17:14 These shall make war with the Lamb, and the Lamb shall overcome them: for he is Lord of lords, and King of kings: and they that are with him are called, and chosen, and faithful.
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>