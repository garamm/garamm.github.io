---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 4-3. 수식 트리"
author: 임가람
date: "2021-08-21 16:25:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 수식 트리(Expression Tree)란
수식을 표현하는 이진 트리<br>
- 피연산자는 잎 노드이다.
- 연산자는 루트 노드, 또는 가지 노드이다.
<br>
1*2+(7-8)은 다음과 같은 수식 트리로 표현 가능<br>
![수식트리-예제](/assets/img/posts/2021-08-21-expression-tree-1.png){: width="500" height="150" }
<br>

---
## 수식 트리의 구축
후위 표기식을 이용해 다음과 같은 과정으로 트리를 구축한다.<br>
- 수식을 뒤에서부터 앞쪽으로 읽어나간다.
- 수식에서 제일 마지막에 있는 토큰은 루트 노드가 된다. 후위 표기식에서 가장 마지막에 있는 토큰은 항상 연산자이다.
- 수식에서 읽어낸 토큰이 연산자인 경우에는 가지 노드가 되며, 이 토큰 다음에 따라오는 두개의 토큰은 각각 오른쪽 자식 노드와 왼쪽 자식 노드가 된다. 단, 다음 토큰에도 연속해서 연산자인 경우에는 이 토큰으로부터 만들어지는 하위 트리가 완성된 이후에 읽어낸 토큰이 왼쪽 자식 노드가 된다.
- 수식에서 읽어낸 토큰이 숫자이면 이 노드는 잎 노드이다.
<br>

예시<br>
(1) 중위 표기식을 후위 표기식으로 변환한다.<br>
1 * 2 + ( 7 - 8 )  → 1 2 * 7 8 - +<br>
<br>
(2) 뒤에서부터 앞쪽으로 읽어나간다. 첫번째 토큰이 연산자이므로 루트 노드에 둔다.<br>
![수식트리구축-1](/assets/img/posts/2021-08-21-expression-tree-2.png){: width="300" height="150" }
<br>
(3) 다음 토큰이 '-' 연산자 이므로 오른쪽 자식 노드로 둔다.<br>
![수식트리구축-2](/assets/img/posts/2021-08-21-expression-tree-3.png){: width="300" height="150" }
<br>
(4) '-' 노드는 연산자 노드이므로 양쪽 자식으로 피연산자를 둔다. 다음 토큰은 숫자 '8'이고, 그 다음 토큰은 숫자 '7'이므로 각각 오른쪽, 왼쪽 자식으로 둔다.<br>
![수식트리구축-3](/assets/img/posts/2021-08-21-expression-tree-4.png){: width="300" height="150" }
<br>
(5) 다음 토큰은 연산자 '*' 이므로 '+' 노드의 왼쪽 자식 노드로 만든다.<br>
![수식트리구축-4](/assets/img/posts/2021-08-21-expression-tree-5.png){: width="300" height="150" }
<br>
(6) '*' 노드는 연산자 노드이므로 양쪽 자식으로 피연산자를 둔다. 다음 토큰은 숫자 '2'이고, 그 다음 토큰은 숫자 '1'이므로 각각 오른쪽, 왼쪽 자식으로 둔다.<br>
![수식트리구축-5](/assets/img/posts/2021-08-21-expression-tree-6.png){: width="300" height="150" }
<br>

---
## 수식 트리의 구현
### 수식 트리 구축하기
매개 변수로 입력된 후위 표기식을 뒤부터 읽어 토큰 하나를 분리한다.<br>
읽어낸 토큰이 연산자라면 방금 읽어낸 연산자 토큰을 노드에 연결하고, 뒤 따라오는 피연산자 둘을 양쪽 자식으로 연결한다.<br>
처음 읽은 토큰이 피연산자인 경우에는 해당 토큰을 노드에 입력하고 함수를 반환한다.
<br>

### 수식 트리 계산하기
노드에 담겨 있는 토큰이 연산자일 경우 노드 양쪽 자식에 대해 재귀 호출하고, 재귀 호출한 함수가 값을 반환하면 왼쪽 수식 트리의 계산 결과는 Left 변수에, 오른쪽 수식 트리의 계산 결과는 Right 변수에 저장한ㄷ.<br>
토큰이 피연산자일 경우엔 토큰에 담겨 있던 값을 수로 변환해서 반환한다.
<br>

---
## 소스코드

ExpressionTree.h
```c
#ifndef EXPRESSION_TREE_H
#define EXPRESSION_TREE_H

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef char ElementType;

typedef struct tagETNode 
{
    struct tagETNode* Left;
    struct tagETNode* Right;

    ElementType Data;
} ETNode;

ETNode*   ET_CreateNode( ElementType NewData );
void      ET_DestroyNode( ETNode* Node );
void      ET_DestroyTree( ETNode* Root );

void      ET_PreorderPrintTree( ETNode* Node );
void      ET_InorderPrintTree( ETNode* Node );
void      ET_PostorderPrintTree( ETNode* Node );
void      ET_BuildExpressionTree( char* PostfixExpression, ETNode** Node );
double    ET_Evaluate( ETNode* Tree );

#endif EXPRESSION_TREE_H
```
ExpressionTree.c
```c
#include "ExpressionTree.h"

ETNode* ET_CreateNode( ElementType NewData )
{
    ETNode* NewNode = (ETNode*)malloc( sizeof(ETNode) );
    NewNode->Left    = NULL;
    NewNode->Right   = NULL;
    NewNode->Data    = NewData;

    return NewNode;
}

void ET_DestroyNode( ETNode* Node )
{
    free(Node);
}

void ET_DestroyTree( ETNode* Root )
{
    if ( Root == NULL )
        return;

    ET_DestroyTree( Root->Left );
    ET_DestroyTree( Root->Right );
    ET_DestroyNode( Root );
}

void ET_PreorderPrintTree( ETNode* Node )
{
    if ( Node == NULL )
        return;

    printf( " %c", Node->Data );

    ET_PreorderPrintTree( Node->Left );
    ET_PreorderPrintTree( Node->Right );
}

void ET_InorderPrintTree( ETNode* Node )
{
    if ( Node == NULL )
        return;
    
    printf( "(" );
    ET_InorderPrintTree( Node->Left );

    printf( "%c", Node->Data );

    ET_InorderPrintTree( Node->Right );
    printf( ")" );
}

void ET_PostorderPrintTree( ETNode* Node )
{
    if ( Node == NULL )
        return;
    
    ET_PostorderPrintTree( Node->Left );
    ET_PostorderPrintTree( Node->Right );
    printf( " %c", Node->Data );
}

void ET_BuildExpressionTree( char* PostfixExpression, ETNode** Node )
{
    ETNode* NewNode = NULL;
    int  len        = strlen( PostfixExpression );
    char Token      = PostfixExpression[ len -1 ];
    PostfixExpression[ len-1 ] = '\0';

    switch ( Token ) 
    {
        /*  연산자인 경우 */
        case '+': case '-': case '*': case '/':
            (*Node) = ET_CreateNode( Token );
            ET_BuildExpressionTree( PostfixExpression, &(*Node)->Right );
            ET_BuildExpressionTree( PostfixExpression, &(*Node)->Left  );
            break;

        /*  피연산자인 경우 */
        default :
            (*Node) = ET_CreateNode( Token);
            break;
    }
}

double ET_Evaluate( ETNode* Tree )
{
    char Temp[2];
    
    double Left   = 0;
    double Right  = 0;
    double Result = 0;

    if ( Tree == NULL )
        return 0;

    switch ( Tree->Data ) 
    {
        /*  연산자인 경우 */
        case '+': case '-': case '*': case '/':
            Left  = ET_Evaluate( Tree->Left );
            Right = ET_Evaluate( Tree->Right );

            if ( Tree->Data == '+' ) Result = Left + Right;
            else if ( Tree->Data == '-' ) Result = Left - Right;
            else if ( Tree->Data == '*' ) Result = Left * Right;
            else if ( Tree->Data == '/' ) Result = Left / Right;            

            break;

        /*  피연산자인 경우 */
        default :
            memset(Temp, 0, sizeof(Temp));
            Temp[0] = Tree->Data;
            Result = atof(Temp);  
            break;
    }

    return Result;
}
```
Test_ExpressionTree.c
```c
#include "ExpressionTree.h"

int main( void )
{
    ETNode* Root = NULL;

    char PostfixExpression[20] = "71*52-/";
    ET_BuildExpressionTree( PostfixExpression, &Root);

    /*  트리 출력 */
    printf("Preorder ...\n");
    ET_PreorderPrintTree( Root );
    printf("\n\n");

    printf("Inorder ... \n");
    ET_InorderPrintTree( Root );
    printf("\n\n");

    printf("Postorder ... \n");
    ET_PostorderPrintTree( Root );
    printf("\n");

    printf("Evaulation Result : %f \n", ET_Evaluate( Root ) );

    /*  트리 소멸시키기 */
    ET_DestroyTree( Root );

    return 0;
}
```
실행결과
```
Preorder ...
 / * 7 1 - 5 2
 
Inorder ...
(((7)*(1))/((5)-(2)))

Postorder ...
 7 1 * 5 2 - /
Evaulation Result : 2.333333
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>