---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 3-1. 큐 / 순환 큐"
author: 임가람
date: "2021-08-17 22:45:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 큐(Queue)
- 먼저 들어간 요소가 먼저 나오는 자료구조 (선입선출, FIFO; First In First Out)

<br>

---
<br>

## 큐의 주요 기능: 삽입과 제거
큐의 가장 앞 요소를 전단(Front)이라고 하고, 마지막 요소를 후단(Rear)이라고 한다.<br>
큐에서 삽입(Enqueue)은 후단에 노드를 덧붙혀 새로운 후단을 만드는 연산이다.<br>
큐에서 제거(Dequeue)는 전단의 노드를 없애서 전단 뒤에 있는 노드를 새로운 전단으로 만드는 연산이다.
![큐-삽입과제거](/assets/img/posts/2021-08-17-queue-circular-1.png){: width="600" height="150" }

<br>

---
<br>

## 순환 큐



<br>

---
<br>

## 예제 코드

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