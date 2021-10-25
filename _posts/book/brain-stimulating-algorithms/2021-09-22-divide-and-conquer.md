---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 12. 분할 정복"
author: 임가람
date: "2021-09-22 17:13:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 분할 정복 알고리즘
적군을 아군보다 작은 규모로 쪼개고, 상대적으로 규모가 작아진 적군들을 상대로 싸우는 것을 분할 정복(Divide and Conquer) 전술이라고 한다.<br>
분할 정복 알고리즘은 문제를 더 이상 나눌 수 없을 때까지 나누고, 나누어진 문제들을 각각 풀어 결국 전체 문제의 답을 얻는 알고리즘이다.(예시. 퀵 정렬)<br>
분할 정복의 핵심은 '문제를 잘게 쪼개기'이다. 문제를 쪼개는 것은 정해진 공식이나 규칙은 없지만 대강의 요령은 다음과 같다.<br>
1. 분할(Divide): 문제가 분할이 가능한 경우, 2개 이상의 하위 문제로 나눈다.
2. 정복(Conquer): 하위 문제가 여전히 분할이 가능한 상태라면 하위 집합에 대해 1번을 수행하고, 그렇지 않으면 하위 문제를 푼다.
3. 결합(Combine): 2번 과정에서 정복된 답을 취합한다.

분할 정복 기법으로 문제를 나눌 때 생기는 각 하위 문제는 완전히 독립적이다. 그러므로 병렬 처리가 가능하다. 분할 정복 기법에는 재귀 호출이 많이 사용하는데, 이 때문에 문제를 잘게 나눔으로써 얻어진 알고리즘의 효율성을 재귀 호출 비용이 깎아내린다는 단점이 있다.
<br>

---
## 분할 정복의 응용
### 병합 정렬(Merge Sort)
퀵 정렬과 같이 분할 정복에 기반한 알고리즘이다. 병합 정렬은 퀵 정렬과 마찬가지로 수행 속도가 굉장히 빠르다. 병합 정렬은 다음과 같은 순서로 동작한다.<br>
1. 정렬할 데이터 집합을 반으로 나눈다.
2. 나눠진 하위 데이터 집합의 크기가 2 이상이면 이 하위 데이터 집합에 대해 1번을 반복한다.
3. 원래 같은 집합에서 나눠져 나온 하위 데이터 집합 둘을 병합하여 하나로 만든다. 단, 병합할 때 데이터 집합의 원소는 순서에 맞춰 정렬한다.
4. 데이터 집합이 다시 하나가 될 떄까지 3번을 반복한다.

병합 정렬 알고리즘을 구현할 때 하위 데이터 집합을 병합하는 부분은 병합 정렬의 성능을 좌우하므로 아주 중요하다.<br>
다음은 병합 정렬의 예시이다.<br>
![병합정렬 1](/assets/img/posts/2021-09-22-divide-and-conquer-1.png){: width="700" height="150" }

우선 데이터 집합을 반으로 나누기 위해 1~2번 과정을 반복한다. 아래 그림은 원본 데이터 집합을 하위 데이터 집합으로 나누는 과정을 나타낸다. 어두운 색으로 표시된 요소는 데이터 집합의 중간에 위치한 기준점을 의미한다.<br>
![병합정렬 2](/assets/img/posts/2021-09-22-divide-and-conquer-2.png){: width="700" height="150" }

분할이 끝났으니 정복 할 차례이다. 알고리즘의 3~4번을 반복해서 수행하되, 나눠졌던 하위 데이터 집합들이 다시 하나가 되면 알고리즘을 종료한다.<br>
![병합정렬 3](/assets/img/posts/2021-09-22-divide-and-conquer-3.png){: width="700" height="150" }
<br>

두 데이터 집합을 정렬하며 합치는 방법은 다음과 같다.<br>
1. 두 데이터 집합을 합한 것만큼의 비어 있는 데이터 집합을 마련한다.
2. 두 데이터 집합의 첫 번째 요소들을 비교하여 작은 요소를 새 데이터 집합에 추가한다. 그리고 새 데이터 집합에 추가된 요소는 원래의 데이터 집합에서 삭제한다.
3. 양쪽 데이터 집합이 빌 때까지 2번의 과정을 반복한다.

예를 들어 아래의 그림에 두 데이터 집합 A, B가 있고, 이들을 합한 크기와 같은 크기를 갖는 빈 데이터 집합 C가 있다. 다음은 A와 B를 병합해서 C에 넣는 방법이다.<br>
![병합정렬 4](/assets/img/posts/2021-09-22-divide-and-conquer-4.png){: width="700" height="150" }

먼저 A와 B의 첫 번쨰 요소끼리 비교를 수행한다. A의 요소 1이 더 작으므로 C에 1을 입력하고 A에서 1을 삭제한다.<br>
![병합정렬 5](/assets/img/posts/2021-09-22-divide-and-conquer-5.png){: width="700" height="150" }

A의 요소 4와 B의 요소 2를 비교한다. B의 요소가 더 작으므로 C에 2를 입력하고 B에서 2를 삭제한다.<br>
![병합정렬 6](/assets/img/posts/2021-09-22-divide-and-conquer-6.png){: width="700" height="150" }

A의 요소 4와 B의 요소 3을 비교한다. B의 요소가 더 작으므로 C에 3을 입력하고 B에서 3을 삭제한다.<br>
![병합정렬 7](/assets/img/posts/2021-09-22-divide-and-conquer-7.png){: width="700" height="150" }

A의 요소 4와 B의 요소 7을 비교한다. A의 요소가 더 작으므로 C에 4를 입력하고 A에서 4를 삭제한다.<br>
![병합정렬 8](/assets/img/posts/2021-09-22-divide-and-conquer-8.png){: width="700" height="150" }

이런 방식으로 계속 하다 보면 B에 9 하나만 남게 된다. A에는 비교할 데이터가 남아있지 않으므로 9를 C에 입력하고 B에서 삭제한다.
![병합정렬 9](/assets/img/posts/2021-09-22-divide-and-conquer-9.png){: width="700" height="150" }
<br>

### 병합 정렬 알고리즘의 구현
MergeSort.c
```c
#include <stdio.h>
#include <stdlib.h>

void MergeSort(int DataSet[], int StartIndex, int EndIndex);
void Merge(int DataSet[], int StartIndex, int MiddleIndex, int EndIndex );

void MergeSort(int DataSet[], int StartIndex, int EndIndex)
{
    int MiddleIndex;

    if ( EndIndex - StartIndex < 1 )
        return;
    
    MiddleIndex = (StartIndex + EndIndex) / 2; /* 절반으로 나누기 위해 중간 위치 찾기 */
    
    MergeSort( DataSet, StartIndex,    MiddleIndex );
    MergeSort( DataSet, MiddleIndex+1, EndIndex ); /* 데이터 집합을 반으로 나눠 다시 병합 정렬 수행 */
    
    Merge( DataSet, StartIndex, MiddleIndex, EndIndex ); /* 다시 병합 */
}

void Merge(int DataSet[], int StartIndex, int MiddleIndex, int EndIndex )
{
    int i;
    int LeftIndex  = StartIndex;
    int RightIndex = MiddleIndex + 1;
    int DestIndex  = 0;

    int* Destination = (int*) malloc( sizeof(int) * (EndIndex - StartIndex + 1) );
    
    while ( LeftIndex <= MiddleIndex && RightIndex <= EndIndex ) 
    {
	    if (DataSet[ LeftIndex ] < DataSet [RightIndex ]) 
        {
		    Destination[ DestIndex ] = DataSet[ LeftIndex ];
		    LeftIndex++;
	    } else {
		    Destination[ DestIndex ] = DataSet[ RightIndex];
		    RightIndex++;
	    }
	    DestIndex++;
    }

    while ( LeftIndex <= MiddleIndex ) 
	    Destination[ DestIndex++ ] = DataSet[ LeftIndex++ ];

    while ( RightIndex <= EndIndex )
        Destination[ DestIndex++ ] = DataSet[ RightIndex++ ];

    DestIndex = 0;
    for (i = StartIndex; i<=EndIndex; i++) 
    {
	    DataSet[i] = Destination[ DestIndex++ ];
    }

    free( Destination );
}

int main( void )
{
    int DataSet[] = {334, 6, 4, 2, 3, 1, 5, 117, 12, 34};
    int Length = sizeof DataSet / sizeof DataSet[0];    
    int i = 0;

    MergeSort( DataSet, 0, Length-1 );

    for ( i=0; i<Length; i++ )
    {
        printf("%d ", DataSet[i]);
    }

    printf("\n");

    return 0;
}
```
실행 결과
```
1 2 3 4 5 6 12 34 117 334
```
위 코드의 MergeSort() 함수는 메모리 공간을 아끼고 메모리 할당에 소요되는 비용을 없애기 위해 하위 데이터 집합을 위한 메모리 및 배열을 따로 할당하지 않는 대신 각 하위 데이터 집합을 구분하는 시작, 중간, 마지막 인덱스를 입력받아 어디서부터 어디까지 작업할 지 식별한다.<br>
<br>

---
## 거듭 제곱(Exponentiation)
거듭 제곱 $C^2$를 계산 하기 위해서는 다음과 같이 $C$ 자신을 두 번 곱해야 한다.<br>
$C^2 = C \times C$ <br>
$C^n$를 계산 하기 위해서는 $C$ 자신을 n 번 곱해야 한다.<br>
$C^n = C \times C \times \ldots	\times C$ <br>
거듭 제곱을 C 코드로 바꾸면 다음과 같다.<br>
```c
int Power( int Base, int Exponent )
{
    int i=0;
    int Result = 1; /* C^0은 1 이므로 */

    for( i=0; i<Exponent; i++ )
        Result += Base;

    return Result;
}
```
매개 변수 Exponent(지수)의 크기만큼 곱셈을 수행하므로 이 알고리즘은 Exponent()에 대해 $O(n)$의 수행 시간을 소요한다. 이것을 더 개선할 수 있는 방법은 다음과 같다.<br>
우선 $C^8 = C \times C \times C \times C \times C \times C \times C$ 로 정의하지만 다음과 같이 표현할 수도 있다.<br>
$C^8 = C^4 \times C^4 = (C^4)^2 = ((C^2)^2)^2$<br>
즉 $C^8$ 을 구할 떄 C를 일곱 번 곱하지 않고 $C^2$ 를 구한 뒤 제곱을 두 번 반복하여 총 세 번의 곱셈을 수행해서 같은 결과를 얻을 수 있다. 그러나 이 방식은 지수가 홀수인 경우 2로 나누어 떨어지지 않기 때문에 모든 멱승 연산에 적용할 수 있는 것은 아니다.<br>
제곱수가 홀수인 경우에는 다음의 정리를 이용한다.<br>
$C^n = C^{(n-1)/2} \times C^{(n-1)/2} \times C = (C^{(n-1)/2})^2 \times C$ <br>
위 정리에 의해 $C^5$ 를 구하면 다음과 같다.<br>
$C^5 = C^{(5-1)/2} \times C^{(5-1)/2} \times C = C^2 \times C^2 \times C = (C^2)^2 \times C$ <br>
위 내용을 정리하여 지수가 짝수/홀수인 경우에 대해 재귀 방정식을 만들면 다음과 같다.<br>

$
C^n =
\begin{cases}
C^{n/2}C^{n/2},\;n은 짝수 \\\\\\
C^{(n-1)/2}C^{(n-1)/2}C,\;n은 홀수
\end{cases}
$

이렇게 지수를 반으로 나눠가는 거듭 제곱 알고리즘의 수행 시간은 $O(\log_{2}{n})$ 이다.
<br>

### 거듭 제곱 알고리즘의 구현
거듭 제곱 연산의 결과는 기하급수적으로 증가하기 때문에 결과를 위한 자료형으로 unsigned long을 사용한다. 아래 예제에서는 typedef 키워드를 이용해 ULONG라는 별칭을 줬다.<br>
FastExponentiation.c
```c
#include <stdio.h>

typedef unsigned long ULONG;

ULONG Power( int Base, int Exponent )
{
    if ( Exponent == 1 )
        return Base;
    else if ( Base == 0 )
        return 1;

    if ( Exponent % 2 == 0 )
    {
        ULONG NewBase = Power( Base, Exponent/2 );
        return NewBase * NewBase;        
    }
    else
    {
        ULONG NewBase = Power( Base, (Exponent-1)/2 );
        return (NewBase * NewBase) * Base;
    }
}

int main( void )
{
    int Base     = 2;
    int Exponent = 30;
    printf("%d^%d = %lu\n", Base, Exponent, Power( Base, Exponent ));

    return 0;
}
```
실행 결과
```
2^30 = 1073741824
```
<br>

---
## 피보나치 수
첫 번째 달에 토끼 한 쌍이 태어났다. 그리고 이 토끼는 두 달이 지나면 다 자라서 번식이 가능하며, 토끼 한 쌍은 매달 새끼 한 쌍을 낳는다고 가정하자. 그럼 8달 후에는 21마리의 토끼가 있게된다.<br>
토끼가 다 자라기까지는 두 달이 걸리므로 두 달까지는 토끼 수에 아무 변화가 없다. 세 달째가 되면 이 토끼들이 새끼 한 쌍을 낳고, 네 달째에는 최초의 토끼가 또 한 쌍의 새끼를 낳지만, 세 달째에 태어난 새끼들은 아직 다 자라지 않았으므로 토끼 수의 총합은 3쌍이 된다. 이런 식으로 8달째까지의 토끼가 번식하는 과정을 전개해보면 아래 표와 같다.<br>

|달 수|토끼의 수(단위: 쌍)|
|---|---|
|1|1|
|2|1|
|3|2|
|4|3|
|5|5|
|6|8|
|7|13|
|8|21|

이 문제의 답 1, 1, 2, 3, 5, 8, 13, $\ldots$ 의 수열을 일컬어 `피보나치 수열`이라고 부르고 피보나치 수열 속의 각 수는 다음과 같은 재귀 방정식으로 정의된다.<br>

$
F_n =
\begin{cases}
0,\;n=0 \\\\\\
1,\;n=1 \\\\\\
F_{n-1}+F_{n-2},\;n>1
\end{cases}
$

이 정의에 의해 0번부터 20번까지의 피보나치 수를 나열해 보면 다음과 같다.<br>

|$F_1$|$F_2$|$F_3$|$F_4$|$F_5$|$F_6$|$F_7$|$F_8$|$F_9$|$F_{10}$|$F_{11}$|$F_{12}$|$F_{13}$|$F_{14}$|$F_{15}$|$F_{16}$|$F_{17}$|$F_{18}$|$F_{19}$|$F_{20}$|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|1|1|2|3|5|8|13|21|34|55|89|144|233|377|610|987|1597|2584|4181|6765|

<br>

### 피보나치 수 구하기
피보나치 수열을 찾는 코드는 아래와 같다.<br>
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
위 소스코드는 Divide and Conquer가 아니라 Double and Conquer 방식으로 문제 해결에 접근하고 있다. 재귀 호출을 전개해보면 다음과 같다.<br>
Fibonacci(n)을 호출하면 두 개의 재귀 호출 Fibonacci(n-1), Fibonacci(n-2)이 발생한다.<br>
![피보나치수열 1](/assets/img/posts/2021-09-22-divide-and-conquer-10.png){: width="700" height="150" }

Fibonacci(n-1)은 Fibonacci(n-2)와 Fibonacci(n-3)의 호출을 생성하고, Fibonacci(n-2)는 Fibonacci(n-3)과 Fibonacci(n-4)의 호출을 생성한다. 따라서 두 번째 단계에서는 Fibonacci() 호출이 네 개로 늘어난다.<br>
![피보나치수열 2](/assets/img/posts/2021-09-22-divide-and-conquer-11.png){: width="700" height="150" }
위 그림 같이 n이 0이 될 때까지 계속 반복한다. 따라서 이 알고리즘이 n번째 피보나치 수를 구하기 위한 수행 시간은 $\Theta(2^n)$ 임을 알 수 있다. $\Theta(2^n)$ 이라면 20번째 피보나치 수를 찾기 위해 100만 회가 넘는 재귀 호출을 해야 한다.<br>
분할 정복 기법을 이용하면 피보나치 수를 찾는 데에 드는 소요 시간을 $\Theta(\log_{2}{n})$ 까지 줄일 수 있다.<br>
<br>

### 분할 정복으로 피보나치 수열 찾기
정사각 행렬 A에 대해서는 $A^nA^m = A^{n+m}$ 이 성립한다. 특히 $n=m$ 인 경우 $A^nA^n = A^{2n}$ 이 된다. 이러한 행렬의 성질을 사용하면 분할 정복으로 피보나치 수를 구할 수 있다.<br>
n번째 피보나치 수를 $F_n$ 이라고 할 때, 아래와 같은 2차원 정사각 행렬을 만들 수 있다.<br>

$
\begin{bmatrix} F_2 & F_1 \\\\\\ F_1 & F_0 \end{bmatrix}
 = \begin{bmatrix} 1 & 1 \\\\\\ 1 & 0 \end{bmatrix}
$

이 정사각 행렬을 n번 제곱하면 다음과 같은 등식을 유도할 수 있다.<br>

$
\begin{bmatrix}F_{n+1}&F_n \\\\\\ F_n&F_{n-1}  \end{bmatrix}
 = \begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}^n
 = \begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}^{n-1}
 \begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}
 $

이 행렬을 계속 $n$회 제곱하면 $O(n)$ 의 피보나치 수 알고리즘을 구현할 수 있지만 정사각 행렬 A에 대해 $A^{n} A^{m} = A^{nm}$ 이고, $n=m$인 경우에는 $A^{n} A^{n} = A^{2n}$ 이라는 성질을 이용하면 더 나은 성능을 끌어낼 수 있다. 위 행렬은 다음과 같이 나타낼 수 있다.<br>

$
\begin{bmatrix}F_{n+1}&F_n \\\\\\ F_n&F_{n-1} \end{bmatrix}
 = \begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}^{n/2}
\begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}^{n/2}
 = \begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}^{n}
$

이 성질을 이용하면 n번째 피보나치 수를 구할 때 $\dfrac{n}{2}$ 번째 피보나치 수를 찾아 제곱하면 되고, $\dfrac{n}{2}$번째 피보나치 수를 구하려면 $\dfrac{n/2}{2}$번째 피보나치 수를 제곱하면 된다. 이것은 곧 n번째 피보나치 수를 구하기 위해서는 최초의 행렬을 $\log_{2}{n}$회 제곱하면 된다는 것을 의미한다.<br>
예를 들어 10번째 피보나치 수를 찾는다고 하면<br>

$
\begin{bmatrix}F_{10+1}&F_{10} \\\\\\ F_{10}&F_{10-1} \end{bmatrix}
 = \begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}^{10/2}
 \begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}^{10/2}
 $

10/2는 5인데, 5는 2로 나누어 떨어지지 않으므로 
$\begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}^n
 = \begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}^{n-1}
 \begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}
$
 의 성질을 이용하여 아래 식을 끌어낸다.<br>

$
\begin{bmatrix}F_{5+1}&F_{5} \\\\\\ F_{5}&F_{5-1} \end{bmatrix}
= \begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}^{5-1}
\begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}
$

다시 제곱수가 짝수가 됐으므로 이제 $\begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}^4$를 구하면 된다.<br>
제곱수를 2로 나눠 다음의 식을 만든다.<br>

$
\begin{bmatrix}F_{4+1}&F_{4} \\\\\\ F_{4}&F_{4-1} \end{bmatrix}
= \begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}^{4/2}
\begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}^{4/2}
$

이번에는 제곱수가 2이다. 이 단계에서는 $\begin{bmatrix}1&1  \\\\\\  1&0 \end{bmatrix}$를 제곱하기만 하면 된다.<br>

$
\begin{bmatrix}F_{2+1}&F_{2} \\\\\\ F_{2}&F_{2-1} \end{bmatrix}
= \begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}
\begin{bmatrix}1&1 \\\\\\ 1&0 \end{bmatrix}
$

이제 분할된 문제를 정복하고 합치면 된다.<br>
$
\begin{bmatrix}1&1 \\\\\\ 1&0\end{bmatrix}
\begin{bmatrix}1&1 \\\\\\ 1&0\end{bmatrix} 
= \begin{bmatrix}2&1 \\\\\\ 1&1\end{bmatrix} 
= \begin{bmatrix}F_{2+1}&F_{2} \\\\\\ F_{2}&F_{2-1}\end{bmatrix}
$
<br>

$
\begin{bmatrix}2&1 \\\\\\ 1&1\end{bmatrix}
\begin{bmatrix}2&1 \\\\\\ 1&1\end{bmatrix} 
= \begin{bmatrix}5&3 \\\\\\ 3&2\end{bmatrix}
= \begin{bmatrix}F_{4+1}&F_{4} \\\\\\ F_{4}&F_{4-1}\end{bmatrix}
$
<br>

$
\begin{bmatrix}5&3 \\\\\\ 3&2\end{bmatrix}
\begin{bmatrix}1&1 \\\\\\ 1&0\end{bmatrix}
= \begin{bmatrix}8&5 \\\\\\ 5&3\end{bmatrix}
= \begin{bmatrix}F_{5+1}&F_{5} \\\\\\ F_{5}&F_{5-1}\end{bmatrix}
$
<br>

$
\begin{bmatrix}8&5 \\\\\\ 5&3\end{bmatrix}
\begin{bmatrix}8&5 \\\\\\ 5&3\end{bmatrix}
= \begin{bmatrix}89&55 \\\\\\ 55&34\end{bmatrix}
= \begin{bmatrix}F_{10+1}&F_{10} \\\\\\ F_{10}&F_{10-1}\end{bmatrix}
$
<br>

위 행렬을 통해 10번재 피보나치 수는 55임을 알 수 있다.<br>
<br>

### 분할 정복 기반의 피보나치 수 알고리즘의 구현
FibonacciDnC.c
```c
#include <stdio.h>

typedef unsigned long ULONG;

typedef struct tagMatrix2x2 /* 2차원 정사각 행렬을 담는 구조체 */
{ 
    ULONG Data[2][2]; 
} Matrix2x2;

Matrix2x2 Matrix2x2_Multiply(Matrix2x2 A, Matrix2x2 B) 
{
    Matrix2x2 C;

    C.Data[0][0] = A.Data[0][0]*B.Data[0][0] + A.Data[0][1]*B.Data[1][0];
    C.Data[0][1] = A.Data[0][0]*B.Data[1][0] + A.Data[0][1]*B.Data[1][1];

    C.Data[1][0] = A.Data[1][0]*B.Data[0][0] + A.Data[1][1]*B.Data[1][0];
    C.Data[1][1] = A.Data[1][0]*B.Data[1][0] + A.Data[1][1]*B.Data[1][1];

    return C;
}

Matrix2x2 Matrix2x2_Power(Matrix2x2 A, int n) 
{
    if( n>1) 
    {
        A = Matrix2x2_Power( A, n/2 ); /* 제곱수를 반으로 나눠 다시 Matrix2x2_Power() 함수를 재귀 호출 */
        A = Matrix2x2_Multiply( A, A );

        if( n & 1 )  
        {
            Matrix2x2 B;    
            B.Data[0][0] = 1; B.Data[0][1] = 1;
            B.Data[1][0] = 1; B.Data[1][1] = 0;

            /* n이 홀수인 경우 n/2를 수행할 때 연산자에 의해 나머지 1이 버려지므로 A에 행렬 (1 1 1 0)을 곱하여 버려진 나머지를 보완 */
            A = Matrix2x2_Multiply( A, B );
        }
    }
    
    return A;
}

ULONG Fibonacci( int N ) 
{
    Matrix2x2 A;    
    /* 행렬 초기화 */
    A.Data[0][0] = 1; A.Data[0][1] = 1;
    A.Data[1][0] = 1; A.Data[1][1] = 0;

    /* 행렬의 N승을 계산 */
    A = Matrix2x2_Power(A, N);

    /* F_n을 반환 */
    return A.Data[0][1];
}

int main( void )
{
    int   N      = 46;
    ULONG Result = Fibonacci( N );

    printf("Fibonacci(%d) = %lu\n", N, Result );

    return 0;
}
```
실행 결과
```
Fibonacci(46) = 1836311903
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>



