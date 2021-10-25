---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 13. 동적 계획법"
author: 임가람
date: "2021-09-22 20:03:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 동적 계획법(Dynamic Programming)
어떤 문제가 여러 단계의 반복되는 부분 문제로 이루어질 때, 각 단계에 있는 부분 문제의 답을 기반으로 전체 문제의 답을 구하는 방법<br>

|분할 정복 기법|동적 계획법|
|---|---|
|문제를 위에서 아래로 나눈다(Top-Down)|제일 작은 부분 문제부터 상위에 있는 문제로 풀어 올라간다.(Bottom Up)|
|각 부분 문제는 완전히 새로운 하나의 문제로 다룰 수 있다|각 단계에 있는 부분 문제들은 그 이전 단계에 있는 문제들의 답에 의존한다|

동적 프로그래밍으로 알고리즘을 설계할 때 부분 문제들의 답을 반복해서 다시 구하지 않도록 부분 문제의 답을 저장해 놓는 테이블을 만들어야 한다.<br>
동적 계획법으로 설계된 알고리즘의 동작 방식은 다음 세 단계로 정리된다.<br>
1. 문제를 부분 문제로 나눈다.
2. 가장 작은 부분 문제부터 해를 구한 뒤 테이블에 저장한다.
3. 테이블에 저장되어 있는 부분 문제의 해를 이용하여 점차적으로 상위 부분 문제의 최적해를 구한다.

동적 계획법을 이용하여 문제를 풀기 위해서는 그 문제가 `최적 부분 구조(Optimal Substructure)`를 갖추고 있다는 전제가 필요하다.<br>
최적 부분 구조란, 전체 문제의 최적해가 부분 문제의 최적해로부터 만들어지는 구조를 말한다. 예를 들어 5개의 작은 문제로 쪼갤 수 있는 어떤 문제가 있고, 쪼개진 문제의 해 5개를 모두 얻어야 이 문제의 해를 구할 수 있다면 이 문제는 최적 부분 구조를 갖추었다고 할 수 있다.<br>

---
## 피보나치 수 구하기
### 동적 계획법으로 피보나치 수 구하기
피보나치 수는 다음의 점화식으로 정리할 수 있다.<br>
$
F_n =
\begin{cases}
0,\;n=0 \\\\\\
1,\;n=1 \\\\\\
F_{n-1}+F_{n-2},\;n>1
\end{cases}
$

점화식을 C 코드로 옮기면 다음과 같다. 이 알고리즘의 수행시간은 $O(2^n)$으로 효율적이지 않다.<br>
```c
ULONG Fibonacci( int n ) 
{
    if ( n == 0 )
        return 0;

    if ( n == 1 || n == 2 )
        return 1;
    
    return Fibonacci(n - 1) + Fibonacci(n - 2);
}
```
이 알고리즘은 부분 피보나치 수를 얻기 위한 중복 계산이 수없이 일어나게된다.<br>
![동적 계획법으로 피보나치 수 구하기](/assets/img/posts/2021-09-22-dynamic-programming-1.png){: width="700" height="150" }

피보나치 문제를 동적 계획법을 이용하여 알고리즘을 재구성하면 $O(2^n)$의 성능을 $O(n)$으로 끌어올릴 수 있다.<br>
동적 계획법으로 이 문제를 풀려면 최적 부분 구조로 이루어져 있는지 확인해야 한다. 피보나치 수는 부분 문제의 답으로부터 본 문제의 답을 얻을 수 있으므로 최적 부분 구조로 이주어져 있다.<br>
이 절차를 피보나치 수 문제에 적용해보면 Fibonacci(n) 함수는 Fibonacci(n-1)과 Fibonacci(n-2)의 합이다. Fibonacci(n-1)은 Fibonacci(n-2)와 Fibonacci(n-3)의 합이다. 그러므로 Fibonacci(n)이라는 문제가 Fibonacci(n-1),  Fibonacci(n-2), $\ldots$ Fibonacci(2), Fibonacci(1), Fibonacci(0)의 부분 집합으로 나뉜다는 사실을 알 수 있다.<br>
부분 문제로 나누는 일이 끝났으면 가장 작은 부분 문제부터 해를 구해 결과를 테이블에 저장한다.<br>

|테이블 인덱스|저장되어 있는 값|
|---|---|
|[0]|0|
|[1]|1|
|[2]|1|
|[3]|2|
|[4]|3|
|[5]|5|
|[6]|8|
|[7]|13|
|[8]|21|
|[9]|34|
|[10]|55|
|$\ldots$|$\ldots$|
|[0]|Fibonacci(n)|

Fibonacci(0)과 Fibonacci(1)은 정의되어 있는 값을 테이블의 0번과 1번 인덱스에 넣고, Fibonacci(2)부터 미리 테이블에 저장되어 있는 값을 이용하여 테이블을 완성해 나간다. Fibonacci(2)는 테이블의 0번과 1번에 저장되어 있는 값을 더해서 그 결과를 2번 인덱스에 저장한다. 이런 식으로 Fibonacci(n)을 구할 때까지 테이블의 값을 참조해서 계산을 반복한다.<br>

### 동적 계획법으로 설계된 피보나치 수 알고리즘의 구현
FibonacciDP.c
```c
#include <stdio.h>
#include <stdlib.h>

typedef unsigned long ULONG;

ULONG Fibonacci( int N ) 
{
    int    i;
    ULONG  Result;
    ULONG* FibonacciTable;

    if ( N==0 || N==1 ) /* N이 0이면 0, 1이면 1을 반환 */
        return N;

    FibonacciTable = (ULONG*)malloc( sizeof( ULONG ) * (N+1) );

    FibonacciTable[0] = 0;
    FibonacciTable[1] = 1;

    for ( i=2; i<=N; i++ ) /* 테이블의 2번부터 N번째 요소까지 차례대로 피보나치 수를 계산하여 입력 */
    {        
        FibonacciTable[i] = FibonacciTable[i-1] + FibonacciTable[i-2];
    }
    
    Result = FibonacciTable[N]; /* N번째 요소가 결과 값이므로 이를 나중에 반환할 Result 변수에 저장 */

    free( FibonacciTable );

    return Result;
}

int main( void )
{
    int   N      = 46;
    ULONG Result = Fibonacci( N );

    printf("Fibonacci(%d)  = %lu \n", N, Result );

    return 0;
}
```
실행 결과
```
Fibonacci(46) = 1836311903
```

---
## 최장 공통 부분 순서(LCS)
### 최장 공통 부분 순서란
순서(Sequence)는 어떤 물건이나 객체의 목록을 가리키는 용어이다. 그리고 이 순서(정렬되어 있는 객체의 목록)에서 일부 요소를 제거한 것은 '부분 순서(Subsequence)'라고 한다.<br>
예를 들어 `GOOD MORNING`이 순서라고 한다면, `OMIG`는 `GOOD MORNING`에서 `G, O, D, D, R, N, N`을 제거한 것이므로 `OMIG`는 이 순서의 부분 순서라고 할 수 있다.<br>
공통 부분 순서는 두 순서 사이에 공통적으로 존재하는 부분 순서를 말한다. `GOOD MORNING`과 `GUTEN MORGEN`의 공통 부분 순서를 보면 `G, GO, GON, GMORN` 등이 나온다. 최장 공통 부분 순서는 이 여러 개의 공통 부분 순서 중에 가장 긴 것을 의미한다.<br>
최장 공통 부분 순서 알고리즘은 주로 두 데이터를 비교할 때 아주 유용하다.<br>

## LCS 알고리즘
두 문자열 $X_i = \langle x_1x_2x_3 \ldots x_i \rangle$ , $Y_j = \langle y_1y_2y_3 \ldots y_j \rangle$이 있다고 하고, 두 문자열을 매개 변수로 받아 이들 사이에서 나올 수 있는 LCS의 '길이'를 구하는 함수는 LCS_LENGTH()라고 하자.<br>
먼저 $i$나 $j$ 둘 중 하나의 길이가 0이면 공통 부분 순서가 존재하지 않기 때문에 LCS_LENGTH($X_i$, $Y_j$)의 결과는 0이다. <br>
그리고 $x_i$와 $y_j$가 같다면, 즉 $X_i$와$Y_j$의 마지막 요소가 같다면 당연히 LCS에 포함되므로 LCS_LENGTH($X_i$, $Y_j$)는 LCS_LENGTH($X_{i-1}$, $Y_{j-1})$+1와 동일하다.<br>
따라서 이 경우에는 다음과 같은 점화식이 성립한다.<br>
LCS_LENGTH($X_i$, $Y_j$) = LCS_LENGTH($X_{i-1}$, $Y_{j-1})$+1<br>
두 문자열의 길이가 둘 다 0이 아니고 마지막 요소도 서로 다른 경우에는 LCS_LENGTH($X_{i-1}$, $Y_j$)와 LCS_LENGTH($X_{i}$, $Y_{j-1}$) 중에서 큰 쪽이 LCS_LENGTH($X_{i}$, $Y_j$)의 해가 된다.<br>
두 문자열의 마지막 요소를 각각 $x_i$, $y_j$라고 하면, 이 두 요소끼리는 같지 않지만 $x_i$와 $y_{j-1}$는 같을 수 있다. 또는 $x_{i-1}$와 $y_{j}$도 같을 수 있다.<br>
따라서 이 경우에는 LCS_LENGTH($X_{i-1}$, $Y_j$)와 LCS_LENGTH($X_{i}$, $Y_{j-1}$) 중에서 큰 쪽이 LCS_LENGTH($X_{i}$, $Y_j$)의 해가 된다.<br>
위 세 가지 경우를 정리하여 점화식을 만들면 다음과 같다.<br>

$
\texttt{LCS_LENGTH} (X_i, Y_j) 
=\begin{cases}
0,\quad i=0 \ or \ j=0 \texttt{LCS_LENGTH} (X_{i-1}, Y_{j-1})+1,\quad x_i=y_j \\\\\\
MAX(\texttt{LCS_LENGTH} (X_{i-1}, Y_j), \texttt{LCS_LENGTH} (X_{i}, Y_{j-1})),\quad i,j \geq 0 \ and \ x_i \neq y_j
\end{cases}
$

위 점화식은 아래와 같이도 정의가 가능하다. 이 점화식은 $i \times j$의 테이블이 있다고 했을 때, TABLE[$i, j$]는 LCS의 길이이며, TABLE의 각 요소는 TABLE[$i, j$]의 부분 문제들의 답을 담고있다는 것을 설명한다.<br>

$
TABLE[i, j] =
\begin{cases}
0,\quad i=0 \ or \ j=0 \\\\\\
TABLE[i-1, j-1]+1,\quad x_i=y_j \\\\\\
MAX(TABLE[i-1, j], TABLE[i, j-1]),\quad i,j \geq 0 \ and \ x_i \neq y_j
\end{cases}
$

위 점화식에서 $i=0, j=0$은 배열 인덱스 처럼 첫 번쨰 요소를 가리키는 것이 아니라 '아무 것도 없음'을 나타낸다. 문자열 $X$의 첫 번째 요소는 $i=1$, 문자열 $Y$의 첫 번째 요소는 $j=1$이다.
<br>
LCSDC.c

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

typedef struct structLCSTable
{
    int** Data;
} LCSTable;

int LCS( char* X, char* Y, int i, int j, LCSTable* Table )
{                                       /* Table의 크기 = (X의 길이 + 1) + (Y의 길이 + 1) */
                                        /* 아무 것도 없는 행(i=0)과 열(j=0)을 표현하기 위해 각 길이에 1을 더해줌 */
    if ( i == 0 || j == 0 )
    {
        Table->Data[i][j] = 0;
        return Table->Data[i][j];
    }
    else if ( X[i-1] == Y[j-1] )
    {
        Table->Data[i][j] = LCS( X, Y, i-1, j-1, Table ) + 1;
        return Table->Data[i][j];
    }
    else 
    {
        int A = LCS( X, Y, i-1, j  , Table );
        int B = LCS( X, Y, i  , j-1, Table );

        if ( A > B )
            Table->Data[i][j] = A;
        else
            Table->Data[i][j] = B;

        return Table->Data[i][j];
    }
}

void LCS_PrintTable( LCSTable* Table, char* X, char* Y, int LEN_X, int LEN_Y )
{
    int i = 0;
    int j = 0;

    printf("%-04s", " ");

    for ( i=0; i<LEN_Y+1; i++ )
        printf("%c ", Y[i]);
    printf("\n");

    for ( i=0; i<LEN_X+1; i++ )
    {   
        printf("%c ", X[i-1]);

        for ( j=0; j<LEN_Y+1; j++ )
        {
            printf("%d ", Table->Data[i][j]);
        }

        printf("\n");
    }
}

int main( void )
{
    char* X = "GOOD MORNING.";
    char* Y = "GUTEN MORGEN.";

    int   LEN_X = strlen(X);
    int   LEN_Y = strlen(Y);

    int   i = 0;
    int   j = 0;
    int   LCSLength = 0;

    LCSTable Table;
    
    Table.Data = (int**)malloc( sizeof(int*) * (LEN_X + 1) );

    for ( i=0; i<LEN_X+1; i++ ) {
        Table.Data[i] = (int*)malloc( sizeof(int) * (LEN_Y + 1) );
        memset( Table.Data[i], 0, sizeof( int) * (LEN_Y + 1 ) );
    }

    LCSLength = LCS(X, Y, LEN_X, LEN_Y, &Table );

    LCS_PrintTable( &Table, X, Y, LEN_X, LEN_Y );

    return 0;
}
```
위 함수가 'GOOD MORNING'과 'CUTEN MORGEN'으로부터 얻어낸 LCS 테이블의 결과는 다음과 같다. 이 테이블의 오른쪽 아래 모서리에 있는 마지막 요소는 두 문자열 사이의 LCS의 길이가 저장된다. 테이블 안쪽에는 X와 Y의 부분 문자열에 대한 LCS의 길이들로 채워진다. 가령 'GOOD MO'와 'GUTEN MO' 사이에 존재하는 LCS의 길이는 아래의 테이블에서 4('G', ' ', 'M', 'O')임을 알 수 있다.<br>
![동적계획법 1](/assets/img/posts/2021-09-22-dynamic-programming-2.png){: width="700" height="150" }

LCS() 함수는 자기 자신을 재귀 호출하는 분기문의 블록을 두 군데 갖고 있고, 그 중에서 마지막 블록(i와 j가 모두 0보다 크고, $X[i-1] != Y[j-1]$인 경우)에서는 함수를 두 번 재귀 호출 한다. 이런 구조의 재귀 호출은 피보나치 수의 예 처럼 $O(2^{n+m})$의 수행 시간을 갖는다.<br>
하지만 동적 계획법을 이용해서 알고리즘을 재구성하면 수행 시간을 $O(nm)$ 수준으로 낮출 수 있다.<br> 
<br>

### 동적 계획법으로 설계하는 LCS 알고리즘
LCS 문제에서는 LCS 테이블의 각 요소가 각 부분 문제이다. 그리고 테이블의 오른쪽/아래 방향으로 내려나면서 나타나는 요소들이 부분 문제를 포함하는 상위 문제이다. 가장 오른쪽 아래 모서리에 있는 요소가 전체 문제이다. 따라서 테이블의 왼쪽/위 모서리부터 시작해서 오른쪽/아래로 내려가면서 계산을 수행하면 가장 작은 문제부터 해를 구하고 이를 기반으로 상위 문제의 해를 구하게 되며, 가장 오른쪽/아래 모서리에 이르면 전체 LCS 길이의 해를 구할 수 있다.<br>
LCS 점화식에 따르면 $i$=0 이거나 $j$=0인 경우에는 $Table[i, j]$=0이다. 따라서 모든 $j$에 대해 $Table[0, j]$를 0으로 만들고 모든 $i$에 대해 0으로 만들면 된다.<br>
![동적계획법 2](/assets/img/posts/2021-09-22-dynamic-programming-3.png){: width="700" height="150" }

그 다음으로 테이블의 1번 행(즉, 두 번째 행)과 1번 열(두 번째 열)부터 LCS 길이를 구해나가면 된다. LCS 길이를 구하는 것은 점화식의 두 번째($X[i]$와 $Y[j]$가 같은 경우)와 세 번째($i, j$가 모두 0보다 크고 $X[i]$와 $Y[j]$가 다른 경우)를 이용하면 된다.<br>
$X[i]$와 $Y[j]$가 같은 경우에는 $Table[i, j]$에  $Table[i-1, j-1]+1$을 대입하면 되고, $i, j$가 모두 0보다 크고 $X[i]$와 $Y[j]$가 다를 때에는 $Table[i-1, j]$나 $Table[i, j-1]$중 큰 값을 $Table[i, j]$에 대입하면 된다. 이렇게 테이블의 왼쪽 위부터 오른쪽 아래까지 계산해 나가면 $X$의 길이를 m, $Y$의 길이를 n이라 했을 때 Table[m, n]을 구하는 데에는 최악의 경우에도 $O(mn)$ 정도의 수행 시간이 소요된다.<br>
![동적계획법 3](/assets/img/posts/2021-09-22-dynamic-programming-4.png){: width="700" height="150" }

지금까지는 LCS의 길이가 아닌 LCS 자체를 구했다. LCS는 LCS() 함수를 이용해서 만든 LCS 테이블에서 얻을 수 있다. LCS 테이블의 오른쪽/아래 모서리부터 왼쪽/위 모서리로 올라가면서 각 LCS 요소를 추적해내야 한다. LCS 테이블에서 LCS를 추적하는 알고리즘은 다음과 같다.<br>
1. 오른쪽/아래 모서리 요소를 시작 셀(Cell)로 지정하고, LCS의 요소를 담기 위한 리스트를 하나 준비한다.
2. 현재 위치한 셀의 값이 왼쪽(←), 왼쪽 위(↖), 위(↑) 셀의 값보다 크면 현재 셀의 값을 리스트의 헤드에 삽입하고 왼쪽 위 셀(↖)로 이동한다.
3. 현재 위치한 셀의 조건이 단계 2에 해당하지 않으며 현재 셀의 값과 왼쪽(←) 셀의 값이 같고 위(↑) 셀의 값보다 큰 경우에는 왼쪽(←)으로 이동한다. 이동만 하고 리스트에 값을 넣지 않는다.
4. 2, 3단계 중 어느 경우에도 해당하지 않는 경우에는 위(↑) 셀로 이동한다. 이동만 하고 리스트에 값을 넣지 않는다.
5. $i$=0 또는 $j$=0이 될 때까지 단계 2~4를 반복한다.

아래 그림은 LCS() 함수가 만든 LCS 테이블인데 테이블의 끝에서부터 추적해오는 예시이다.<br>
![동적계획법 4](/assets/img/posts/2021-09-22-dynamic-programming-5.png){: width="700" height="150" }

<br>

---
## LCS 예제 프로그램
LCSDP.c
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

typedef struct structLCSTable
{
    int** Data;
} LCSTable;
/* LCS 테이블 만들기 */
int LCS( char* X, char* Y, int i, int j, LCSTable* Table )
{
    int m = 0;
    int n = 0;

    /* i=0 또는 j=0일 때는 Table[i, j]를 0으로 초기화 */
    for ( m=0; m<=i; m++ )
        Table->Data[m][0] = 0;

    for ( n=0; n<=j; n++ )
        Table->Data[0][n] = 0;
    
    /* 테이블의 왼쪽 위부터 시작해서 오른쪽 아래까지 답을 구함 */
    for ( m=1; m<=i; m++ ) 
    {
        for ( n=1; n<=j; n++ )
        {
            if ( X[m-1] == Y[n-1] )
                Table->Data[m][n] = Table->Data[m-1][n-1] + 1;
            else
            {
                if ( Table->Data[m][n-1] >= Table->Data[m-1][n] )
                    Table->Data[m][n] = Table->Data[m][n-1];
                else
                    Table->Data[m][n] = Table->Data[m-1][n];
            }
        }
    }
    
    return Table->Data[i][j];
}
/* LCS 테이블 추적 */
void LCS_TraceBack( char* X, char* Y, int m, int n, LCSTable* Table, char* LCS)
{
    if ( m == 0 || n == 0 ) /* m=0 또는 n=0이면 종료 */
        return;
    
    /* 현재 셀이 왼쪽(←), 왼쪽 위(↖), 위(↑) 셀보다 크면 문자열 앞에 현재 셀의 내용 삽입 후, 왼쪽 위(↖) 셀로 이동 */
    if ( Table->Data[m][n] > Table->Data[m][n-1] 
        && Table->Data[m][n] > Table->Data[m-1][n]
        && Table->Data[m][n] > Table->Data[m-1][n-1] )
    {
        char TempLCS[100];
        strcpy( TempLCS, LCS );
        sprintf(LCS, "%c%s", X[m-1], TempLCS );

        LCS_TraceBack( X, Y, m-1, n-1, Table, LCS );
    }
    /* 현재 셀이 위(↑) 셀보다 크고 왼쪽(←)과 같은 값일 때 왼쪽(←)으로 이동 */
    else if ( Table->Data[m][n] > Table->Data[m-1][n] 
                && Table->Data[m][n] == Table->Data[m][n-1])
    {
        LCS_TraceBack( X, Y, m, n-1, Table, LCS );
    }
    else
    {
        LCS_TraceBack( X, Y, m-1, n, Table, LCS ); /* 위(↑)로 이동 */
    }
}

void LCS_PrintTable( LCSTable* Table, char* X, char* Y, int LEN_X, int LEN_Y )
{
    int i = 0;
    int j = 0;

    printf("%4s", "");

    for ( i=0; i<LEN_Y+1; i++ )
        printf("%c ", Y[i]);
    printf("\n");

    for ( i=0; i<LEN_X+1; i++ )
    {   
        if ( i == 0 )
            printf( "%2s", "");
        else
            printf("%-2c", X[i-1]);

        for ( j=0; j<LEN_Y+1; j++ )
        {
            printf("%d ", Table->Data[i][j]);
        }

        printf("\n");
    }
}

int main( void )
{
    char* X = "GOOD MORNING.";
    char* Y = "GUTEN MORGEN.";
    char* Result;

    int   LEN_X = strlen(X);
    int   LEN_Y = strlen(Y);

    int   i = 0;
    int   j = 0;
    int   Length = 0;

    LCSTable Table;
    
    Table.Data = (int**)malloc( sizeof(int*) * (LEN_X + 1) );

    for ( i=0; i<LEN_X+1; i++ ) {
        Table.Data[i] = (int*)malloc( sizeof(int) * (LEN_Y + 1) );
        
        memset( Table.Data[i], 0, sizeof( int) * (LEN_Y + 1 ) );
    }

    Length = LCS(X, Y, LEN_X, LEN_Y, &Table );
    
    LCS_PrintTable( &Table, X, Y, LEN_X, LEN_Y );

    Result = (char*)malloc( sizeof( Table.Data[LEN_X][LEN_Y] + 1 ) );
    sprintf( Result, "" );

    LCS_TraceBack(X, Y, LEN_X, LEN_Y, &Table, Result);

    printf("\n");
    printf("LCS:\"%s\" (Length:%d)\n", Result, Length);

    return 0;
}
```
실행 결과
```
    G U T E N   M O R G E N .
  0 0 0 0 0 0 0 0 0 0 0 0 0 0
G 0 1 1 1 1 1 1 1 1 1 1 1 1 1
O 0 1 1 1 1 1 1 1 2 2 2 2 2 2
O 0 1 1 1 1 1 1 1 2 2 2 2 2 2
D 0 1 1 1 1 1 1 1 2 2 2 2 2 2
  0 1 1 1 1 1 2 2 2 2 2 2 2 2
M 0 1 1 1 1 1 2 3 3 3 3 3 3 3
O 0 1 1 1 1 1 2 3 4 4 4 4 4 4
R 0 1 1 1 1 1 2 3 4 5 5 5 5 5
N 0 1 1 1 1 2 2 3 4 5 5 5 6 6
I 0 1 1 1 1 2 2 3 4 5 5 5 6 6
N 0 1 1 1 1 2 2 3 4 5 5 5 6 6
G 0 1 1 1 1 2 2 3 4 5 6 6 6 6
. 0 1 1 1 1 2 2 3 4 5 6 6 6 7

LCS:"G MORN." (Length:7)
```
<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>