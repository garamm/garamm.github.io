---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 8. 해시 테이블"
author: 임가람
date: "2021-09-07 21:24:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 해시(Hash)
데이터를 입력받으면 완전히 다른 모습의 데이터로 바꾸어 놓는 작업<br>
해시의 용도
- 해시 테이블: 데이터의 해시 값을 테이블 내의 주소로 이용하는 궁극의 탐색 알고리즘, 이진 탐색보다 빠르다.
- 암호화: 해시를 통해 변환된 데이터는 원본과 완전히 달라지기 때문에 암호화에서 쓰인다. ex. SHA 알고리즘
- 데이터 축약: 길이가 서로 다른 입력 데이터에 대해 일정한 길이의 출력을 만들 수 있기 때문에 커다란 데이터를 짧은 길이로 축약할 수 있다. 축약된 데이터끼리 비교를 수행하면 커다란 원본 데이터들을 비교하는 것에 비해 엄청난 효율을 얻을 수 있다.
<br>

---
## 해시 테이블(Hash Table)
![해시테이블-정의1](/assets/img/posts/2021-09-07-hash-table-1.png){: width="700" height="150" }<br>
다음과 같은 데이터 구조에서 123817이라는 데이터가 있는 요소에 접근하기 위해서는 탐색을 해야한다.<br>
일반적인 경우라면 아주 짧은 시간 내에 탐색을 완료할 수 있다. 그러나 금융분야의 경우 엄청난 트래픽을 받아들여 데이터를 분석 및 처리해야 한다. 그래서 이진 탐색 대신 해시 테이블을 사용한다.<br>
해시 테이블은 다음과 같이 사용한다.<br>
![해시테이블-정의2](/assets/img/posts/2021-09-07-hash-table-2.png){: width="700" height="150" }<br>
123817을 해시(잘개 쪼개고 다시 뭉쳐서)하여 주소값 3819을 구하고, 다음과 같이 Table 내의 해당 주소(인덱스)에 데이터를 저장하면 된다.<br>
```c
Table[3819] = 123817;
```
테이블에 저장되어 있는 데이터를 읽기 위해서는 주소값 3819를 이용해 Table에서 꺼내 쓰면 된다.
```c
printf( Table[3819] ); /* 123817 출력 */
```
이 과정을 정리하면 다음과 같다.<br>
![해시테이블-정의3](/assets/img/posts/2021-09-07-hash-table-3.png){: width="700" height="150" }<br>
*** 데이터를 담을 테이블을 미리 크게 확보해 놓은 후 입력받은 데이터를 해시하여 테이블 내의 주소를 계산하고 이 주소에 데이터를 담는다. ***
<br>
해시 테이블은 데이터가 입력되지 않은 여유 공간이 많아야 제 성능을 유지할 수 있다.<br>
![해시테이블-정의4](/assets/img/posts/2021-09-07-hash-table-4.png){: width="700" height="150" }<br>

<br>

---
## 해시 함수(Hash Function)
입력 값에서 테이블 내의 주소를 계산한다.<br>

### 나눗셈법(Division Method)
해시 함수 중에서도 가장 간단한 알고리즘<br>
입력한 값을 테이블의 크기로 나누고, 그 '나머지'를 테이블의 주소로 사용<br>
```c
주소 = 입력 값 % 테이블의 크기
```
어떤 값이든 테이블의 크기로 나누면 그 나머지는 테이블의 크기 $n$을 넘지 않는다.<br>
입력 값이 데이블의 크기의 배수 또는 약수인 경우 해시 함수는 0을 반환하고, 그렇지 않으면 최대 $n-1$을 반환한다.<br>
즉, 나눗셈법은 0부터 $n-1$ 사이의 주소를 반환함을 보장한다.<br>
```c
int Hash(int Input, int TableSize) {
    return Input % TableSize;
}
```
예시로 테이블의 크기가 193일 때 데이터가 9인 경우 9를 반환하고, 418일 때는 32, 1031일 때는 66, 27일 때는 27을 반환한다.<br>
![해시테이블-나눗셈법](/assets/img/posts/2021-09-07-hash-table-5.png){: width="700" height="150" }<br>
<br>
일반적으로 나눗셈법으로 구현된 해시 함수가 테이블 내의 공간을 효율적으로 사용하기 위해서는 테이블의 크기 $n$을 소수(Prime Number)로 정하는 것이 좋다. (특히 2의 제곱수과 거리가 먼 소수를 사용한 해시 함수)<br>
위에서 사용한 예시 193은 $2^7=128$과 $2^8=256$ 사이에 있는 소수이다.

|앞|뒤|소수|
|---|---|---|
|$2^5$|$2^6$|53|
|$2^6$|$2^7$|97|
|$2^7$|$2^8$|193|
|$2^8$|$2^9$|389|
|$2^9$|$2^10$|769|
|$2^10$|$2^11$|1543|
|$2^11$|$2^12$|3079|
|$2^12$|$2^13$|6151|
|$2^13$|$2^14$|12289|
|$2^14$|$2^15$|24593|
|...|...|...|

### 나눗셈법 예제 프로그램

SimpleHashTable.h
```c
#ifndef SIMPLE_HASHTABLE_H
#define SIMPLE_HASHTABLE_H

#include <stdio.h>
#include <stdlib.h>

typedef int KeyType;
typedef int ValueType;

typedef struct tagNode
{
    KeyType   Key;      /* 주소로 사용할 데이터 */
    ValueType Value;    /* Key를 이용하여 얻어낸 주소에 저장할 데이터 */
} Node;

typedef struct tagHashTable 
{
    int   TableSize;
    Node* Table;
} HashTable;

HashTable* SHT_CreateHashTable( int TableSize );
void       SHT_Set( HashTable* HT, KeyType Key, ValueType Value );
ValueType  SHT_Get( HashTable* HT, KeyType Key );
void       SHT_DestroyHashTable( HashTable* HT);
int        SHT_Hash( KeyType Key, int TableSize );

#endif
```
SimpleHashTable.c
```c
#include "SimpleHashTable.h"

HashTable* SHT_CreateHashTable( int TableSize )
{
    HashTable* HT = (HashTable*)malloc( sizeof(HashTable) );
    HT->Table     = (Node*)malloc( sizeof(Node) * TableSize);
    HT->TableSize = TableSize;

    return HT;
}

void SHT_Set( HashTable* HT, KeyType Key, ValueType Value )
{
    int Address = SHT_Hash( Key, HT->TableSize );

    HT->Table[Address].Key   = Key;
    HT->Table[Address].Value = Value;
}

ValueType SHT_Get( HashTable* HT, KeyType Key )
{
    int Address = SHT_Hash( Key, HT->TableSize );

    return HT->Table[Address].Value;
}

void SHT_DestroyHashTable( HashTable* HT)
{
    free ( HT->Table );
    free ( HT );
}

int SHT_Hash( KeyType Key, int TableSize )
{
    return Key % TableSize;
}
```
Test_SimpleHashTable.c
```c
#include "SimpleHashTable.h"

int main( void )
{
    HashTable* HT = SHT_CreateHashTable( 193 );

    SHT_Set( HT, 418,   32114);
    SHT_Set( HT, 9,    514);
    SHT_Set( HT, 27,   8917);
    SHT_Set( HT, 1031, 286);

    printf("Key:%d, Value:%d\n", 418,  SHT_Get( HT, 418 ) );
    printf("Key:%d, Value:%d\n", 9,    SHT_Get( HT, 9 ) );
    printf("Key:%d, Value:%d\n", 27,   SHT_Get( HT, 27 ) );
    printf("Key:%d, Value:%d\n", 1031, SHT_Get( HT, 1031 ) );

    SHT_DestroyHashTable( HT );

    return 0;
}
```
실행결과
```
Key:417, Value:32114
Key:9, Value:514
Key:27, Value:8917
Key:1031, Value:286
```
<br>

### 자릿수 접기(Digits Folding))
서로 다른 입력 값에 대해 동일한 해시 값. 즉, 해시 테이블 내의 동일한 주소를 반환할 가능성이 높다. 이것을 충돌(Collision)이라고 하는데, 똑같은 해시 값이 아니더라도 해시 테이블 내의 일부 지역의 주소들을 집중적으로 반환하는 결과로 데이터들이 한 곳에 모이는 문제(클러스터(Cluster)라고 함)가 발생할 가능성도 높다.<br>
이런 문제가 발생할 가능성을 줄인 알고리즘 여러개 줄 하나가 자릿수 접기이다.<br>
자릿수 접기란 종이를 접듯이 숫자를 접어 일정한 크기 이하의 수로 만드는 방법이다.<br>
<br>
예를 들어 아래와 같이 7자리의 숫자가 있다고 할 때
```
8 1 2 9 3 3 5
```
이 수의 각 자릿수를 더하면 새로운 값 31이 나온다.
```
31 = 8 + 1 + 2 + 9 + 3 + 3 + 5
```
이렇게 해서 8129335가 31로 접혀졌다.
<br>
두 자리씩 더하면 다음과 같다.
```
148 = 81 + 29 + 33 + 5
```

이처럼 숫자의 각 자릿수를 더해 해시 값을 만드는 것을 자릿수 접기라고 한다.<br>
10진수의 경우 모든 수는 각 자리마다 0~9까지의 값을 가질 수 있으니 7자리 수에 대해 '한 자리씩 접기'를 하면 최소 0에서 최대 63(9+9+9+9+9+9+9)까지의 해시 값을 얻을 수 있고, '두 자리씩 접기'를 하면 최소 0에서 최대 306(99+99+99+9)까지의 해시 값을 얻을 수 있다.<br>
자릿수 접기는 문자열을 키로 사용하는 해시 테이블에 특히 잘 어울린다. 문자열의 각 요소를 ASCII 코드 번호로 바꾸고, 이 값들을 각각 더하여 접으면 문자열을 깔끔하게 해시 테이블 내의 주소로 바꿀 수 있다.<br>
![해시테이블-자릿수접기1](/assets/img/posts/2021-09-07-hash-table-6.png){: width="700" height="150" }<br>
문자열 키를 자릿수 접기 알고리즘을 통해 해시 값을 만들어 내는 코드
```c
int Hash(char* Key, int KeyLength, int TableSize) {
    int i=0;
    int HashValue = 0;

    /* 문자열의 각 요소를 ASCII 코드 번호로 변환하여 더한다. */
    for(i=0; i<KeyLength; i++)
        HashValue += Key[i];

    return HashValue % TableSize;
}
```
그런데 해시 테이블의 크기가 12289이고 문자열 키의 최대 길이가 10자리라고 한다면, 해시 함수는 $10 \times 127=1270$ 이므로 0에서 1270 사이의 주소만을 반환하게 되어 1271~12288 사이의 주소가 전혀 활용되지 않게 된다. ASCII 코드로 10자리를 만들었을 때 조합할 수 있는 경우의 수가 $127^{10}$가지나 된다.<br>
테이블의 크기 12289를 2진수로 표현하면 11000000000001(14비트)이다. 반면에, 위 코드에 있는 Hash() 함수가 반환하는 최대의 주소값은 1270(이진수: 10011110110)은 11비트만을 사용한다. 즉 테이블의 주소 중에서 3개의 비트는 절대 사용되지 않는다.<br>
절대 사용하지 않는 3개의 비트를 모두 활용한다면 해시 테이블의 문제점을 해결할 수 있다.<br>
![해시테이블-자릿수접기2](/assets/img/posts/2021-09-07-hash-table-7.png){: width="700" height="150" }<br>

아래의 Hash()함수는 루프가 반복될 때마다 HashValue를 왼쪽으로 3비트씩 밀어올린 다음 ASCII 코드 번호를 더한다. 이렇게 하면 Hash() 함수는 이론적으로 해시 테이블 내의 모든 주소를 해싱할 수 있다.<br>
```c
int Hash(char* Key, int KeyLength, int TableSize) {
    int i=0;
    int HashValue = 0;

    for(i=0; i<KeyLength; i++)
        HashValue += (HashValue << 3) + Key[i];

    return HashValue % TableSize;
}
```

### 해시 함수의 한계: 충돌
해시 함수가 서로 다른 입력 값에 대해 동일한 해시 테이블 주소를 반환하는 것을 충돌(Collision)이라고 한다.<br>
자릿수 접기 해시 함수도 테이블의 크기가 12289일 때 WJLY와 SZSR 둘 다 10871을 반환하고, KFRE와 SNRZ 둘 다 4508을 반환하고, QBPL과 AUVW도 둘 다 7973을 반환한다.<br>
<br>

---
## 충돌 해결하기
- 개방 해싱(Open Hashing): 해시 테이블의 주소 바깥에 새로운 공간을 할당하여 문제를 수습하기
- 폐쇄 해싱(Closed Hashing): 처음에 주어진 해시 테이블의 공간 안에서 문제를 해결

---
## 체이닝(Chaining)
해시 함수에서 충돌이 발생하면 각 데이터를 해당 주소에 있는 링크드 리스트에 삽입하여 문제를 해결하는 기법<br>
체이닝 기반의 해시 테이블은 데이터 대신 링크드 리스트에 대한 포인터를 관리한다.<br>
데이터들은 해시 테이블의 각 요소가 가리키고 있는 링크드 리스트에 저장된다.<br>
이렇게 해시 테이블의 외부에 데이터를 저장하는 체이닝은 개방 해싱 알고리즘이다.<br>
![해시테이블-체이닝](/assets/img/posts/2021-09-07-hash-table-8.png){: width="700" height="150" }<br>

### 체이닝 기반 해시 테이블의 탐색
탐색 연산은 '충돌이 발생했을' 것을 고려해서 설계되어야 한다.
1. 찾고자 하는 목표값을 해싱하여 링크드 리스트가 저장되어 있는 주소를 찾는다.
2. 이 주소를 이용하여 해시 테이블에 저장되어 있는 링크드 리스트에 대한 포인터를 생성한다.
3. 링크드 리스트의 앞에서부터 뒤까지 차례대로 이동하여 목표값이 저장되어 있는지 비교한다. 목표값과 링크드 리스트 내의 노드 값이 일치하면 해당 노드의 주소를 반환한다.

### 체이닝 기반 해시 테이블의 삽입
해시 함수를 이용해서 해시 데이터가 삽입될 링크드 리스트의 주소를 얻어낸 후에, 링크드 리스트가 비어 있으면 데이터를 바로 삽입하고, 그렇지 않으면 링크드 리스트의 '헤드 앞에' 삽입한다.<br>
충돌이 일어나면 우리는 새로운 데이터를 링크드 리스트에 삽입하면 된다. 새로운 데이터를 링크드 리스트의 테일로 만들고 싶으면 '순차 탐색'을 수행하여 테일 노드를 찾아 삽입해야 하는데 같은 주소에서 충돌을 여러번 겪는다고 가정하면 링크드 리스트의 길이가 길어질수록 더 많은 순차 탐색을 해야한다. 그렇기 때문에 헤드 앞에 삽입하면 순차 탐색이 필요 없으므로 헤드에 삽입한다.
<br>

### 체이닝 예제 프로그램
Chaining.h
```c
#ifndef CHAINING_H
#define CHAINING_H

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef char* KeyType;
typedef char* ValueType;

typedef struct tagNode
{
    KeyType   Key;
    ValueType Value;

    struct tagNode* Next;
} Node;

typedef Node* List;

typedef struct tagHashTable 
{
    int    TableSize;
    List*  Table;
} HashTable;

HashTable* CHT_CreateHashTable( int TableSize );
void       CHT_DestroyHashTable( HashTable* HT);

Node*      CHT_CreateNode( KeyType Key, ValueType Value );
void       CHT_DestroyNode( Node* TheNode );

void       CHT_Set( HashTable* HT, KeyType Key, ValueType Value );
ValueType  CHT_Get( HashTable* HT, KeyType Key );
int        CHT_Hash( KeyType Key, int KeyLength, int TableSize );

#endif CHAINING_H
```
Chaining.c
```c
#include "Chaining.h"

HashTable* CHT_CreateHashTable( int TableSize )
{
    HashTable* HT = (HashTable*)malloc( sizeof(HashTable) );
    HT->Table     = (List*)malloc( sizeof(Node) * TableSize);

    memset(HT->Table, 0, sizeof(List) * TableSize );

    HT->TableSize = TableSize;

    return HT;
}

Node* CHT_CreateNode( KeyType Key, ValueType Value )
{
    Node* NewNode = (Node*) malloc( sizeof(Node) );

    NewNode->Key = (char*)malloc( sizeof(char) * ( strlen(Key) + 1 ));
    strcpy( NewNode->Key,   Key );

    NewNode->Value = (char*)malloc( sizeof(char) * ( strlen(Value) + 1 ));
    strcpy( NewNode->Value, Value );
    NewNode->Next = NULL;

    return NewNode;
}

void CHT_DestroyNode( Node* TheNode )
{
    free( TheNode->Key );
    free( TheNode->Value );
    free( TheNode );
}

void CHT_Set( HashTable* HT, KeyType Key, ValueType Value )
{
    int Address = CHT_Hash( Key, strlen(Key), HT->TableSize );
    Node* NewNode = CHT_CreateNode( Key, Value );

    /*  해당 주소가 비어 있는 경우 */
    if ( HT->Table[Address] == NULL )
    {
        HT->Table[Address] = NewNode;
    } 
    /*  해당 주소가 비어 있지 않은 경우 */
    else
    {    
        List L = HT->Table[Address];
        NewNode->Next         = L;
        HT->Table[Address] = NewNode;

        printf("Collision occured : Key(%s), Address(%d)\n", Key, Address );
    }
}

ValueType CHT_Get( HashTable* HT, KeyType Key )
{
    /*  주소를 해싱한다. */
    int Address = CHT_Hash( Key, strlen(Key), HT->TableSize );

    /*  해싱한 주소에 있는 링크드 리스트를 가져온다. */
    List TheList = HT->Table[Address];
    List Target  = NULL;

    if ( TheList == NULL )
        return NULL;

    /*  원하는 값을 찾을 때까지 순차 탐색. */
    while ( 1 )
    {
        if ( strcmp( TheList->Key, Key ) == 0 ) 
        {
            Target = TheList;
            break;
        }
        
        if ( TheList->Next == NULL )
            break;
        else
            TheList = TheList->Next;
    }

    return Target->Value;
}

void CHT_DestroyList( List L )
{
    if ( L == NULL )
        return;

    if ( L->Next != NULL )
        CHT_DestroyList( L->Next );

    CHT_DestroyNode( L );
}

void CHT_DestroyHashTable( HashTable* HT)
{
    /*  1. 각 링크드 리스트를 자유 저장소에서 제거하기 */
    int i = 0;
    for ( i=0; i<HT->TableSize; i++ )
    {
        List L = HT->Table[i];
        
        CHT_DestroyList( L );    
    }

    /*  2, 해시 테이블을 자유 저장소에서 제거하기. */
    free ( HT->Table );
    free ( HT );
}

int CHT_Hash( KeyType Key, int KeyLength, int TableSize )
{
    int i=0;
    int HashValue = 0;

    for ( i=0; i<KeyLength; i++ )
    {
        HashValue = (HashValue << 3) + Key[i];
    }

    HashValue = HashValue % TableSize;

    return HashValue;
}
```
Test_Chaining.c
```c
#include "Chaining.h"

int main( void )
{
    HashTable* HT = CHT_CreateHashTable( 12289 );

    CHT_Set( HT, "MSFT",   "Microsoft Corporation");
    CHT_Set( HT, "JAVA",   "Sun Microsystems");
    CHT_Set( HT, "REDH",   "Red Hat Linux");
    CHT_Set( HT, "APAC",   "Apache Org");
    CHT_Set( HT, "ZYMZZ",  "Unisys Ops Check"); /*  APAC와 충돌 */
    CHT_Set( HT, "IBM",    "IBM Ltd.");
    CHT_Set( HT, "ORCL",   "Oracle Corporation");
    CHT_Set( HT, "CSCO",   "Cisco Systems, Inc.");
    CHT_Set( HT, "GOOG",   "Google Inc.");
    CHT_Set( HT, "YHOO",   "Yahoo! Inc.");
    CHT_Set( HT, "NOVL",   "Novell, Inc.");

    printf("\n");
    printf("Key:%s, Value:%s\n", "MSFT",   CHT_Get( HT, "MSFT" ) );
    printf("Key:%s, Value:%s\n", "REDH",   CHT_Get( HT, "REDH" ) );
    printf("Key:%s, Value:%s\n", "APAC",   CHT_Get( HT, "APAC" ) );
    printf("Key:%s, Value:%s\n", "ZYMZZ",  CHT_Get( HT, "ZYMZZ" ) );
    printf("Key:%s, Value:%s\n", "JAVA",   CHT_Get( HT, "JAVA" ) );
    printf("Key:%s, Value:%s\n", "IBM",    CHT_Get( HT, "IBM" ) );
    printf("Key:%s, Value:%s\n", "ORCL",   CHT_Get( HT, "ORCL" ) );
    printf("Key:%s, Value:%s\n", "CSCO",   CHT_Get( HT, "CSCO" ) );
    printf("Key:%s, Value:%s\n", "GOOG",   CHT_Get( HT, "GOOG" ) );
    printf("Key:%s, Value:%s\n", "YHOO",   CHT_Get( HT, "YHOO" ) );
    printf("Key:%s, Value:%s\n", "NOVL",   CHT_Get( HT, "NOVL" ) );

    CHT_DestroyHashTable( HT );

    return 0;
}
```
실행결과
```
Collision occured : Key(ZYMZZ), Address(2120)

Key:MSFT, Value:Microsoft Corporation
Key:REDH, Value:Red Hat Linux
Key:APAC, Value:Apache Org
Key:ZYMZZ, Value:Unisys Ops Check
Key:JAVA, Value:Sun Microsystems
Key:IBM, Value:IBM Ltd.
Key:ORCL, Value:Oracle Corporation
Key:CSCO, Value:Cisco Systems, Inc.
Key:GOOG, Value:Google Inc.
Key:YHOO, Value:Yahoo! Inc.
Key:NOVL, Value:Novell, Inc.
```
<br>

### 체이닝의 성능을 더 끌어올리려면?
체이닝은 원하는 데이터를 찾기 위해 순차 탐색을 해야 한다. 해시 테이블은 극한의 탐색을 위해 고안되었는데 순차 탐색으로 그 성능이 줄어버린다.<br>
해시 테이블의 극한의 성능에는 못 미치지만, 레드 블랙 트리와 같은 이진 탐색 트리를 사용하면 이 문제를 해결할 수 있다.<br>

---
## 개방 주소법(Open Addressing)
충돌이 일어날 때 해시 함수에 의해 얻어진 주소가 아니더라도 얼마든지 다른 주소를 사용할 수 있도록 허용하는 충돌 해결 알고리즘<br>
체이팅은 해시 함수에 의해 얻어진 주소만 사용하는 기법이기 때문에 오픈 해싱 기법인 동시에 폐쇄 주소법(Closed Addressing) 알고리즘이기도 하다.<br>
개발 주소법은 충돌이 일어나면 해시 테이블 내의 새로운 주소를 탐사(Probe)하여 충돌되 데이터를 입력하는 방식으로 동작한다.<br>

### 선형 탐사(Linear Probing)
가장 간단한 탐사 방법으로, 해시 함수로부터 얻어낸 주소에 이미 다른 데이터가 입력되어 있음을 발견하면, 현재 주소에서 고정 폭(예를 들면 1)으로 다음 주소로 이동한다.<br>
그 주소에도 다른 데이터가 있어 충돌이 발생하면 그 다음 주소로 이동하고 비어있는 주소를 찾을떄까지 반복한다.<br>
<br>

예시로 크기가 13이고 나눗셈법 기반의 해시 함수를 이용하는 해시 테이블이 있다고 가정한다.<br>
여기에 42를 입력하면 42 % 13 = 3 이므로 테이블의 3번째 요소에 입력한다.<br>
![해시테이블-선형탐사1](/assets/img/posts/2021-09-07-hash-table-9.png){: width="700" height="150" }<br>
이어서 55를 입력하면 55 % 13 = 3 인데 이미 이 주소는 42가 사용하고 있어 충돌이 일어난다.<br>
![해시테이블-선형탐사2](/assets/img/posts/2021-09-07-hash-table-10.png){: width="700" height="150" }<br>
이때 선형탐사를 시작해서 충돌이 일어난 주소의 뒤로 1만큼 이동하면 빈 주소가 있기 때문에 거기에 55를 입력한다.<br>
![해시테이블-선형탐사3](/assets/img/posts/2021-09-07-hash-table-11.png){: width="700" height="150" }<br>
이어서 81을 입력하면 81 % 13 = 3 인데 이미 이 주소는 42가 사용하고 있어 충돌이 일어난다.<br>
![해시테이블-선형탐사4](/assets/img/posts/2021-09-07-hash-table-12.png){: width="700" height="150" }<br>
이제 선형탐사를 시작하는데 첫 번째 탐사를 통해 찾은 주소는 이미 55가 입력되어 있으므로 두 번째 탐사를 통해 찾은 비어있는 주소에 81을 입력한다.
![해시테이블-선형탐사5](/assets/img/posts/2021-09-07-hash-table-13.png){: width="700" height="150" }<br>

<br>
위의 예시처럼 해시 테이블에 삽입된 데이터들이 서로 모이게 되는 클러스터(Cluster) 현상이 매우 잘 발생한다.<br>
클러스터가 발생하는 새로운 주소를 찾기 위해 수행하는 선형 탐사 시간이 길어져서 해시 테이블의 성능이 저하된다.<br>
이 문제를 개선하기 위해 제곱 탐사 알고리즘을 사용한다.

### 제곱 탐사(Quadratic Probing)
선형 탐사가 고정폭만큼 이동하는 것에 비해 제곱 탐사는 이동폭이 제곱수로 늘어난다.<br>
<br>

예시로 크기가 13이고 나눗셈법 기반의 해시 함수를 이용하는 해시 테이블이 있다고 가정한다.<br>
여기에 키가 42인 데이터를 입력한다. 42 % 13 = 3 이므로 3번째에 42 데이터를 입력한다.<br>
![해시테이블-제곱탐사1](/assets/img/posts/2021-09-07-hash-table-14.png){: width="700" height="150" }<br>
이어서 55를 입력한다. 55 % 13 = 3이고, 이 주소에는 이미 42가 입력되어 있기 때문에 충돌이 발생한다.<br>
![해시테이블-제곱탐사2](/assets/img/posts/2021-09-07-hash-table-15.png){: width="700" height="150" }<br>
이제 제곱탐사를 수행한다. 제곱 탐사를 처음 수행할 때는 1의 제곱만큼을 이동한다. $1^2$은 1이기 때문에 1만큼만 이동한다.<br>
![해시테이블-제곱탐사3](/assets/img/posts/2021-09-07-hash-table-16.png){: width="700" height="150" }<br>
이어서 81을 입력한다. 81 % 13 = 3이고, 충돌이 발생한다.<br>
![해시테이블-제곱탐사4](/assets/img/posts/2021-09-07-hash-table-17.png){: width="700" height="150" }<br>
다시 제곱 탐사를 수행한다. 첫 번째 1의 제곱수만큼 이동하면 이미 55가 사용하고 있기 때문에 두 번째 탐사를 한다. 두 번째 탐사에서는 $2^2$, 4만큼 이동한다.<br>
![해시테이블-제곱탐사5](/assets/img/posts/2021-09-07-hash-table-18.png){: width="700" height="150" }<br>
그 결과 해시 값이 똑같이 3이 되는 42, 55, 81은 아래와 같이 해시 테이블에 입력된다.<br>
![해시테이블-제곱탐사6](/assets/img/posts/2021-09-07-hash-table-19.png){: width="700" height="150" }<br>

<br>

그런데 제곱 탐사는 충돌이 일어났을 때 '제곱수의 폭'으로 이동한다는 규칙을 가진다. 그리고 이 규칙성에 의해 또 다른 종류의 클러스터 발생한다.<br>
![해시테이블-제곱탐사7](/assets/img/posts/2021-09-07-hash-table-20.png){: width="700" height="150" }<br>
이런 현상이 발생하는 이유는 한 주소에서 충돌이 발생하면 탐사할 위치가 정해져 있기 때문이다.<br>
제곱 탐사는 서로 다른 해시 값을 갖는 데이터에 대해서는 클러스터가 형성되지 않도록 하는 효과가 어느 정도 있지만, 같은 해시 값을 갖는 데이터에 대해 2차 클러스터를 형성하도록 유도하는 문제가 있다.<br>
1차 클러스터와 2차 클러스터를 모두 해결하기 위해 충돌이 일어났을 때 탐사할 새로운 주소에 대한 규칙성을 없애 해결한다.
<br>

### 이중 해싱(Double Hashing)
1차, 2차 클러스터를 방지하기 위해 이중 해싱을 사용한다.<br>
2개의 해시 함수를 준비해서 하나는 최초의 주소를 얻을 때, 다른 하나는 충돌이 일어났을 때 탐사 이동폭을 얻기 위해 사용하면 이동폭의 규칙성은 없애면서도 같은 키에 대해서는 항상 똑같은 결과를 얻을 수 있다.<br>

예시로 첫 번째 해시 함수와 충돌 발생시 탐사 이동폭을 계산하는 두 번쨰 해시를 다음과 같이 정의한다.
```c
int Hash(int Key) {
    return Key % 13;
}
int Hash2(int Key) {
    return (Key % 11) + 2;
}
```

우선 키가 42인 데이터를 입력하면 42 % 13 = 3이고 주소 3에 입력한다.<br>
![해시테이블-이중해싱1](/assets/img/posts/2021-09-07-hash-table-21.png){: width="700" height="150" }<br>

이어서 55를 입력하면 충돌이 발생한다. 그러므로 새로운 주소를 탐사하기 위해 Hash2() 함수를 이용해서 이동폭을 구한다. Hash2()에 매개변수 44를 넘기면 2를 반환한다.<br>
![해시테이블-이중해싱2](/assets/img/posts/2021-09-07-hash-table-22.png){: width="700" height="150" }<br>

55의 새 주소는 원래의 주소 3에 이동폭 2를 더한 5가 된다.<br>
![해시테이블-이중해싱3](/assets/img/posts/2021-09-07-hash-table-23.png){: width="700" height="150" }<br>

이어서 81을 입력하면 충돌이 발생한다. 
![해시테이블-이중해싱4](/assets/img/posts/2021-09-07-hash-table-24.png){: width="700" height="150" }<br>

Hash2()를 사용하여 88의 이동폭을 계산하면 6이므로 새로운 주소는 3 + 6 = 9이다.<br>
![해시테이블-이중해싱5](/assets/img/posts/2021-09-07-hash-table-25.png){: width="700" height="150" }<br>

마지막으로 224를 입력하면 충돌이 발생한다. 그리고 Hash2() 함수를 사용하여 주소 9로 가는데 이 위치도 충돌이 일어난다. 그러므로 다시 6만큼 뒤로 이동하면 주소 15로 간다. 테이블의 마지막 주소 12보다 크므로 다시 앞으로 가면 되므로 224의 새 주소는 2이다.<br>
![해시테이블-이중해싱6](/assets/img/posts/2021-09-07-hash-table-26.png){: width="700" height="150" }<br>

<br>

### 재해싱(Rehashing)
아무리 뛰어난 충돌 해결 알고리즘도 해시 테이블의 여유 공간이 모두 차버려서 발생하는 충돌은 막을 수 없다.<br>
![해시테이블-재해싱1](/assets/img/posts/2021-09-07-hash-table-27.png){: width="700" height="150" }<br>

재해싱은 해시 테이블의 크기를 늘리고, 늘어난 해시 테이블의 크기에 맞추어 테이블 내의 모든 데이터를 다시 해싱한다. 그러면 또 다시 여유 공간이 생기게 된다.<br>
![해시테이블-재해싱2](/assets/img/posts/2021-09-07-hash-table-28.png){: width="700" height="150" }<br>
<br>
통계적으로 해시 테이블의 공간 사용률이 70%~80%에 이르면 성능 저하가 나타나기 시작한다. 그러므로 재해싱을 이 전에 수행해야 성능 저하가 나타나는 것을 막을 수 있다.<br>
하지만 재해싱도 많은 오버헤드를 요구하기 때문에 재해싱을 수행할 임계치를 너무 낮게 잡는 것도 성능 저하의 원인이 되므로 임계치는 75% 정도를 사용하는 것이 보통이다.<br>

### 개방 주소법 예제 프로그램
아래 예제에서는 충돌이 일어났을 때 이중 해싱으로 새 주소를 탐사하고, 데이터가 해시 테이블의 공간을 50% 이상 차지하면 재해싱하여 테이블의 크기를 늘리도록 한다.
<br>

OpenAddressing.h
```c
#ifndef OPEN_ADDRESSING_H
#define OPEN_ADDRESSING_H

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef char* KeyType;
typedef char* ValueType;

enum ElementStatus
{
    EMPTY    = 0,
    OCCUPIED = 1
};

typedef struct tagElementType
{
    KeyType    Key;
    ValueType  Value;

    enum ElementStatus Status;
} ElementType;

typedef struct tagHashTable 
{
    int           OccupiedCount;
    int           TableSize;

    ElementType*  Table;
} HashTable;

HashTable* OAHT_CreateHashTable( int TableSize );
void       OAHT_DestroyHashTable( HashTable* HT );
void       OAHT_ClearElement( ElementType* Element );

void       OAHT_Set( HashTable** HT, KeyType Key, ValueType Value );
ValueType  OAHT_Get( HashTable* HT, KeyType Key );
int        OAHT_Hash ( KeyType Key, int KeyLength, int TableSize );
int        OAHT_Hash2( KeyType Key, int KeyLength, int TableSize );

void       OAHT_Rehash( HashTable** HT );

#endif
```
OpenAddressing.c
```c
#include "OpenAddressing.h"

HashTable* OAHT_CreateHashTable( int TableSize )
{
    HashTable* HT = (HashTable*)malloc( sizeof(HashTable) );
    HT->Table     = (ElementType*)malloc( sizeof(ElementType) * TableSize);

    memset(HT->Table, 0, sizeof(ElementType) * TableSize );

    HT->TableSize = TableSize;
    HT->OccupiedCount = 0;

    return HT;
}

/* 해시 테이블에 데이터를 입력 */
void OAHT_Set( HashTable** HT, KeyType Key, ValueType Value )
{
    int    KeyLen, Address, StepSize;
    double Usage;

    Usage = (double)(*HT)->OccupiedCount / (*HT)->TableSize;

    if ( Usage > 0.5 )
    {
        OAHT_Rehash( HT );
    }

    KeyLen    = strlen(Key);
    Address   = OAHT_Hash( Key, KeyLen, (*HT)->TableSize );
    StepSize  = OAHT_Hash2( Key, KeyLen, (*HT)->TableSize );
    
    while ( (*HT)->Table[Address].Status != EMPTY && 
                strcmp((*HT)->Table[Address].Key, Key) != 0 )
    {
        printf("Collision occured! : Key(%s), Address(%d), StepSize(%d)\n", 
                Key, Address, StepSize );

        Address = (Address + StepSize) % (*HT)->TableSize;
    }

    (*HT)->Table[Address].Key = (char*)malloc( sizeof(char) * (KeyLen + 1) );
    strcpy( (*HT)->Table[Address].Key, Key );

    (*HT)->Table[Address].Value = (char*)malloc( sizeof(char) * (strlen(Value) + 1) );
    strcpy( (*HT)->Table[Address].Value, Value );

    (*HT)->Table[Address].Status = OCCUPIED;

    (*HT)->OccupiedCount++;

    printf("Key(%s) entered at address(%d)\n", Key, Address );
}
/* 해시 테이블에서 데이터를 탐색 */
ValueType OAHT_Get( HashTable* HT, KeyType Key )
{
    int KeyLen    = strlen(Key);

    int Address   = OAHT_Hash( Key, KeyLen, HT->TableSize );
    int StepSize  = OAHT_Hash2( Key, KeyLen, HT->TableSize );
    
    while ( HT->Table[Address].Status != EMPTY && 
                strcmp(HT->Table[Address].Key, Key) != 0 )
    {
        Address = (Address + StepSize) % HT->TableSize;
    }

    return HT->Table[Address].Value;
}

void OAHT_ClearElement( ElementType* Element )
{
    if ( Element->Status == EMPTY)
        return;

    free(Element->Key);
    free(Element->Value);
}

void OAHT_DestroyHashTable( HashTable* HT)
{
    /*  1. 각 링크드 리스트를 자유 저장소에서 제거하기 */
    int i = 0;
    for ( i=0; i<HT->TableSize; i++ )
    {
        OAHT_ClearElement( &(HT->Table[i]) );
    }

    /*  2, 해시 테이블을 자유 저장소에서 제거하기. */
    free ( HT->Table );
    free ( HT );
}

int OAHT_Hash( KeyType Key, int KeyLength, int TableSize )
{
    int i=0;
    int HashValue = 0;

    for ( i=0; i<KeyLength; i++ )
    {
        HashValue = (HashValue << 3) + Key[i];
    }

    HashValue = HashValue % TableSize;

    return HashValue;
}
/* 충돌이 일어났을 때 탐사 이동폭을 계산 */
int OAHT_Hash2( KeyType Key, int KeyLength, int TableSize )
{
    int i=0;
    int HashValue = 0;

    for ( i=0; i<KeyLength; i++ )
    {
        HashValue = (HashValue << 2) + Key[i];
    }

    HashValue = HashValue % ( TableSize - 3 );

    return HashValue + 1;
}
/* 해시 테이블의 크기를 두 배로 늘려 재해싱 */
void OAHT_Rehash( HashTable** HT )
{
    int i = 0;
    ElementType* OldTable = (*HT)->Table;

    /*  새 해시 테이블을 만들고, */
    HashTable* NewHT = OAHT_CreateHashTable( (*HT)->TableSize * 2 );
    
    printf("\nRehashed. New table size is : %d\n\n", NewHT->TableSize );

    /*  이전의 해시테이블에 있던 데이터를 새 해시테이블로 옮긴다. */
    for ( i=0; i<(*HT)->TableSize; i++ )
    {
        if ( OldTable[i].Status == OCCUPIED )
        {
            OAHT_Set( &NewHT, OldTable[i].Key, OldTable[i].Value );
        }
    }

    /*  이전의 해시테이블은 소멸시킨다. */
    OAHT_DestroyHashTable( (*HT) );

    /*  HT 포인터에는 새로 해시테이블의 주소를 대입한다. */
    (*HT) = NewHT;
}
```
Test_OpenAddressing.c
```c
#include "OpenAddressing.h"

int main( void )
{
    HashTable* HT = OAHT_CreateHashTable( 11 );

    OAHT_Set( &HT, "MSFT",   "Microsoft Corporation");
    OAHT_Set( &HT, "JAVA",   "Sun Microsystems");
    OAHT_Set( &HT, "REDH",   "Red Hat Linux");
    OAHT_Set( &HT, "APAC",   "Apache Org");
    OAHT_Set( &HT, "ZYMZZ",  "Unisys Ops Check"); /* APAC와 충돌 */
    OAHT_Set( &HT, "IBM",    "IBM Ltd.");
    OAHT_Set( &HT, "ORCL",   "Oracle Corporation");
    OAHT_Set( &HT, "CSCO",   "Cisco Systems, Inc.");
    OAHT_Set( &HT, "GOOG",   "Google Inc.");
    OAHT_Set( &HT, "YHOO",   "Yahoo! Inc.");
    OAHT_Set( &HT, "NOVL",   "Novell, Inc.");

    printf("\n");
    printf("Key:%s, Value:%s\n", "MSFT",   OAHT_Get( HT, "MSFT" ) );
    printf("Key:%s, Value:%s\n", "REDH",   OAHT_Get( HT, "REDH" ) );
    printf("Key:%s, Value:%s\n", "APAC",   OAHT_Get( HT, "APAC" ) );
    printf("Key:%s, Value:%s\n", "ZYMZZ",  OAHT_Get( HT, "ZYMZZ" ) );
    printf("Key:%s, Value:%s\n", "JAVA",   OAHT_Get( HT, "JAVA" ) );
    printf("Key:%s, Value:%s\n", "IBM",    OAHT_Get( HT, "IBM" ) );
    printf("Key:%s, Value:%s\n", "ORCL",   OAHT_Get( HT, "ORCL" ) );
    printf("Key:%s, Value:%s\n", "CSCO",   OAHT_Get( HT, "CSCO" ) );
    printf("Key:%s, Value:%s\n", "GOOG",   OAHT_Get( HT, "GOOG" ) );
    printf("Key:%s, Value:%s\n", "YHOO",   OAHT_Get( HT, "YHOO" ) );
    printf("Key:%s, Value:%s\n", "NOVL",   OAHT_Get( HT, "NOVL" ) );

    OAHT_DestroyHashTable( HT );

    return 0;
}
```
실행결과
```
Key(MSFT) entered at address(5)
Key(JAVA) entered at address(0)
Key(REDH) entered at address(2)
Key(APAC) entered at address(3)
Key(ZYMZZ) entered at address(10)
Key(IBM) entered at address(8)

Rehashed. New table size is : 22

Key(JAVA) entered at address(11)
Key(REDH) entered at address(2)
Key(APAC) entered at address(3)
Key(MSFT) entered at address(16)
Key(IBM) entered at address(19)
Key(ZYMZZ) entered at address(10)
Key(ORCL) entered at address(20)
Key(CSCO) entered at address(15)
Collision occured! : Key(GOOG), Address(3), StepSize(2)
Key(GOOG) entered at address(5)
Key(YHOO) entered at address(1)
Key(NOVL) entered at address(18)

Key:MSFT, Value:Microsoft Corporation
Key:REDH, Value:Red Hat Linux
Key:APAC, Value:Apache Org
Key:ZYMZZ, Value:Unisys Ops Check
Key:JAVA, Value:Sun Microsystems
Key:IBM, Value:IBM Ltd.
Key:ORCL, Value:Oracle Corporation
Key:CSCO, Value:Cisco Systems, Inc.
Key:GOOG, Value:Google Inc.
Key:YHOO, Value:Yahoo! Inc.
Key:NOVL, Value:Novell, Inc.
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>