---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 2-3. 스택 / 응용: 사칙 연산 계산기"
author: 임가람
date: "2021-08-15 19:17:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 중위 표기법과 후위 표기법
- 중위 표기법: 연산자를 피연산자 가운데에 위치시키는 것, 우리가 일반적으로 사용하는 표기법<br>ex. 1 + 3
- 후위 표기법: 연산자를 피연산자 뒤에 위치시키는 것<br>ex. 1 3 +
- 폴리쉬 표기법(전위 표기법): 연산자를 피연산자 앞에 위치시키는 것<br>+ 1 3

### 후위 표기식을 계산하는 알고리즘
규칙
- 왼쪽부터 요소를 읽으면서 피연산자를 스택에 담는다.
- 연산자가 나타난 경우 피연산자 2개를 스택에서 꺼낸 후 연산을 실행하고, 연산 결과를 다시 스택에 삽입한다.
<br>
예시: 1 3 2 * -<br>
- 요소 1은 피연산자 이므로 스택에 담는다.<br>![스택-계산기-1](/assets/img/posts/2021-08-15-stack-calculator-1.png){: width="300" height="150" }
- 요소 3은 피연산자 이므로 스택에 담는다.<br>![스택-계산기-2](/assets/img/posts/2021-08-15-stack-calculator-2.png){: width="300" height="150" }
- 요소 2은 피연산자 이므로 스택에 담는다.<br>![스택-계산기-3](/assets/img/posts/2021-08-15-stack-calculator-3.png){: width="300" height="150" }
- 요소 *은 연산자 이므로 스택에서 요소 2, 3을 꺼내 연산을 해준 후 다시 스택에 삽입한다.<br>![스택-계산기-4](/assets/img/posts/2021-08-15-stack-calculator-4.png){: width="300" height="150" }
- 요소 -은 연산자 이므로 스택에서 요소 6, 1을 꺼내 연산을 해준 후 다시 스택에 삽입한다.<br>![스택-계산기-5](/assets/img/posts/2021-08-15-stack-calculator-5.png){: width="300" height="150" }
- 식을 모두 읽었기 때문에 스택에 있는 결과 값을 꺼내준다.
<br>
순서도<br>
![스택-계산기-순서도](/assets/img/posts/2021-08-15-stack-calculator-6.png){: width="600" height="150" }

<br>

### 중위 표기법에서 후위 표기법으로 변환하는 알고리즘
1. 입력받은 중위 표기식에서 토큰을 읽는다.
2. 토큰이 피연산자이면 토큰을 결과에 출력한다.
3. 토큰이 연산자(괄호 포함)이면 스택의 최상위 노드에 담겨있는 연산자가 토큰보다 우선 순위가 높은지 검사한다.<br>검사 결과가 참이면 최상위 노드를 스택에서 꺼내 결과에 출력하며, 이 검사 작업을 반복해서 수행하되 그 결과가 거짓이거나 스택이 비게 되면 작업을 중단한다.<br>검사 작업이 끝난 후에는 토큰을 스택에 삽입한다. (스택에는 최상위 노드보다 우선순위가 높은 연산자는 존재하지 않는다.)
4. 토큰이 오른쪽 괄호 ')' 이면 최상위 노드에 왼쪽 괄호 '('가 올 때까지 스택에 제거 연산을 수행하고, 제거한 노드에 담긴 연산자를 출력한다.<br>왼쪽 괄호를 만나면 제거만 하고 출력하지는 않는다.
5. 중위 표기식에 더 읽을 것이 없다면 빠져나가고, 더 읽을 것이 있다면 처음부터 다시 반복한다.<br>
<br>
[예제] (117.32 + 83) * 49를 후위 표기식으로 변환하는 과정<br>
![스택-계산기-후위표기법변환표](/assets/img/posts/2021-08-15-stack-calculator-7.png){: width="600" height="150" }

<br>

---

## 예제 코드
[LinkedListStack.h, LinkedListStack.c 소스코드 보기](/posts/stack-linkedlist/#예제-코드){:target="_blank"}<br>

Calculator.h
```c
#ifndef CALCULATOR_H
#define CALCULATOR_H

#include <stdlib.h>
#include "LinkedListStack.h"

typedef enum
{
    LEFT_PARENTHESIS  = '(', RIGHT_PARENTHESIS = ')',
    PLUS     = '+', MINUS    = '-',
    MULTIPLY = '*', DIVIDE   = '/',
    SPACE    = ' ', OPERAND
} SYMBOL;

int          IsNumber( char Cipher );
unsigned int GetNextToken( char* Expression, char* Token, int* TYPE );
int          IsPrior( char Operator1, char Operator2 );
void         GetPostfix( char* InfixExpression, char* PostfixExpression );
double       Calculate( char* PostfixExpression );

#endif CALCULATOR_H
```
Calculator.c
```c
#include "Calculator.h"

char NUMBER[] = { '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '.' };

int IsNumber( char Cipher )
{
    int i = 0;
    int ArrayLength = sizeof(NUMBER);

    for ( i=0; i<ArrayLength; i++ )
    {
        if ( Cipher == NUMBER[i] )
            return 1;
    }

    return 0;
}

unsigned int GetNextToken( char* Expression, char* Token, int* TYPE )
{
    unsigned int i = 0;
    
    for ( i=0 ; 0 != Expression[i]; i++ )
    {
        Token[i] = Expression[i];

        if ( IsNumber( Expression[i] ) == 1 )
        {            
            *TYPE = OPERAND;

            if ( IsNumber(Expression[i+1]) != 1 )
                break;
        }
        else
        {
            *TYPE = Expression[i];
            break;            
        }
    }

    Token[++i] = '\0';
    return i;
}

int GetPriority(char Operator, int InStack)
{
    int Priority = -1;

    switch (Operator)
    {
    case LEFT_PARENTHESIS:
        if ( InStack )
            Priority = 3;
        else
            Priority = 0;
        break;

    case MULTIPLY:
    case DIVIDE:
        Priority = 1;
        break;

    case PLUS:
    case MINUS:
        Priority = 2;
        break;
    }

    return Priority;
}

int IsPrior( char OperatorInStack, char OperatorInToken )
{
    return ( GetPriority(OperatorInStack, 1) > GetPriority(OperatorInToken, 0) );
}
/* 중위 표기법에서 후위 표기법으로 변환 */
void GetPostfix( char* InfixExpression, char* PostfixExpression )
{ /* InfixExpression; 중위 표기식, PostfixExpression; 후위 표기식 */
    LinkedListStack* Stack;

    char Token[32];
    int  Type = -1;
    unsigned int Position = 0;
    unsigned int Length = strlen( InfixExpression );

    LLS_CreateStack(&Stack);

    while ( Position < Length ) /* 중위 표기식을 다 읽을 때까지 */
    {
        Position += GetNextToken( &InfixExpression[Position], Token, &Type );

        if ( Type == OPERAND ) /* 토큰이 피연산자라면 후위 표기식에 출력 */
        {
            strcat( PostfixExpression, Token );
            strcat( PostfixExpression, " " );
        }
        else if ( Type == RIGHT_PARENTHESIS ) /* 토큰이 오른쪽 괄호라면 왼쪽 괄호가 나타날 때까지 스택의 노드를 제거 */
        {               
            while ( !LLS_IsEmpty(Stack) ) 
            {
                Node* Popped = LLS_Pop( Stack );

                if ( Popped->Data[0] == LEFT_PARENTHESIS )
                {
                    LLS_DestroyNode( Popped );
                    break;
                }
                else /* 토큰이 연산자인 경우 */
                {
                    strcat( PostfixExpression, Popped->Data );          
                    LLS_DestroyNode( Popped );
                }
            }
        }
        else
        {
            while ( !LLS_IsEmpty( Stack ) && 
                    !IsPrior( LLS_Top( Stack )->Data[0], Token[0] ) )
            {
                Node* Popped = LLS_Pop( Stack );

                if ( Popped->Data[0] != LEFT_PARENTHESIS )
                    strcat( PostfixExpression, Popped->Data );
                
                LLS_DestroyNode( Popped );
            }
            
            LLS_Push( Stack, LLS_CreateNode( Token ) );
        }
    }

    while ( !LLS_IsEmpty( Stack) ) /* 중위 표기식을 다 읽었으니 Stack에 남아있는 모든 연산자를 후위 표기식에 출력  */
    {
        Node* Popped = LLS_Pop( Stack );

        if ( Popped->Data[0] != LEFT_PARENTHESIS )
            strcat( PostfixExpression, Popped->Data );
        
        LLS_DestroyNode( Popped );
    }

    LLS_DestroyStack(Stack);
}
/* 후위 표기식을 계산하는 함수 */
double Calculate( char* PostfixExpression )
{
    LinkedListStack* Stack;
    Node*  ResultNode;

    double Result;
    char Token[32];
    int  Type = -1;
    unsigned int Read = 0; 
    unsigned int Length = strlen( PostfixExpression );

    /* Stack 생성 */
    LLS_CreateStack(&Stack); 

    while ( Read < Length ) /* PostfixExpression을 다 읽을 때 까지 */
    {
       /* PostfixExpression에서 토큰을 읽고, 읽은 토큰 크기를 Read에 누적 */
        Read += GetNextToken( &PostfixExpression[Read], Token, &Type );

        if ( Type == SPACE )  
            continue;
        
        if ( Type == OPERAND ) /* 토큰이 피연산자면 스택에 삽입 */
        {
            Node* NewNode = LLS_CreateNode( Token );
            LLS_Push( Stack, NewNode );
        }
        else /* 토큰이 연산자면 스택에서 피연산자 2개를 꺼내서 연산자로 계산 후 스택에 저장 */
        {
            char   ResultString[32];            
            double Operator1, Operator2, TempResult;
            Node* OperatorNode;

            OperatorNode = LLS_Pop( Stack );
            Operator2 = atof( OperatorNode->Data );
            LLS_DestroyNode( OperatorNode );

            OperatorNode = LLS_Pop( Stack );
            Operator1 = atof( OperatorNode->Data );
            LLS_DestroyNode( OperatorNode );
            
            switch (Type) /* 연산자 종류에 따라 계산 */
            {
            case PLUS:     TempResult = Operator1 + Operator2; break;
            case MINUS:    TempResult = Operator1 - Operator2; break;
            case MULTIPLY: TempResult = Operator1 * Operator2; break;
            case DIVIDE:   TempResult = Operator1 / Operator2; break;
            }

            gcvt( TempResult, 10, ResultString ); /* 소수로 되어 있는 계산 결과를 문자열로 변환 */
            LLS_Push( Stack, LLS_CreateNode( ResultString ) );
        }
    }

    ResultNode = LLS_Pop( Stack ); /* 스택에 마지막으로 남아있는 값을 Result에 저장 */
    Result = atof( ResultNode->Data ); /* 문자열로 되어 있는 계산 결과를 소수로 변환 */
    LLS_DestroyNode( ResultNode );

    LLS_DestroyStack( Stack );

    return Result;
}
```
Test_Calculator.c
```c
#include <stdio.h>
#include <string.h>
#include "Calculator.h"

int main( void )
{
    char InfixExpression[100];
    char PostfixExpression[100];

	double Result = 0.0;

    memset( InfixExpression,   0, sizeof(InfixExpression) );
    memset( PostfixExpression, 0, sizeof(PostfixExpression) );
    
    printf( "Enter Infix Expression:" );
    scanf( "%s", InfixExpression );
    
    GetPostfix( InfixExpression, PostfixExpression );
    
    printf( "Infix:%s\nPostfix:%s\n",
            InfixExpression,
            PostfixExpression );

	Result = Calculate( PostfixExpression );

    printf( "Calculation Result : %f\n", Result );

    return 0;
}
```
실행 결과
```
Enter Infix Expression:1+3.334/(4.28*(110-7729))
Infix:1+3.334/(4.28*(110-7729))
Postfix:1 3.334 4.28 110 7729 -*/+
Calculation Result : 0.999898
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>