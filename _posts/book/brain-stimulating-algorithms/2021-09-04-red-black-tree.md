---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 6-4. 탐색 / 레드 블랙 트리"
author: 임가람
date: "2021-09-04 15:47:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 레드 블랙 트리(Red Black Tree)
이진 탐색 트리와 다르게 노드를 빨간색 또는 검은색으로 표시한다.<br>
레드 블랙 트리 노드의 구조체는 다음과 같이 색깔을 위한 필드를 따로 추가해야 한다.<br>
```c
typedef struct tagRBTNode 
{
    struct tagRBTNode* Parent;
    struct tagRBTNode* Left;
    struct tagRBTNode* Right;

    /* 노드의 색을 나타내는 Color 필드 */
    /* RED 아니면 BLACK의 값을 저장할 수 있다. */
    enum { RED, BLACK } Color;    

    ElementType Data;

} RBTNode;
```
<br>

### 레드 블랙 트리가 균형을 유지하는 비결
1. 모든 노드는 빨간색 아니면 검은색이다.
2. 루트 노드는 검은색이다.
3. 잎 노드는 검은색이다.
4. 빨간 노드의 자식들은 모두 검은색이다. 하지만 검은색 노드의 자식이 빨간색일 필요는 없다.
5. 루트 노드에서 모든 잎 노드 사이에 있는 검은색 노드의 수는 모두 동일하다.

![레드블랙트리-구조](/assets/img/posts/2021-09-04-red-black-tree-1.png){: width="700" height="150" }<br>
<br>
위 트리는 유효한 레드 블랙 트리이다.<br>
레드 블랙 트리의 잎 노드는 모두 NIL로 표시되어 있는데 NIL 노드는 아무 데이터도 갖고 있지 않지만 색깔만 검은색인 더미 노드이다.<br>
아무 데이터도 갖고 있지 않지만 레드 블랙 트리를 구현하기 쉽게 하기 위해서 사용한다.<br>
원래의 잎 노드들이 검은색이든 빨간색이든 NIL 노드를 양쪽 자식으로 연결해서 NIL 노드가 잎 노드가 되면 "모든 잎 노드는 검은색이다"라는 규칙을 항상 유지시킬 수 있기 때문이다.<br>
<br>

---
## 레드 블랙 트리의 기본 연산
탐색은 이진 트리와 동일하게 하면 되지만, 삽입과 삭제는 레드 블랙 트리의 규칙이 무너지기 때문에 삽입과 삭제 연산 후에 처리를 잘 해줘야 한다.
<br>

### 회전(Route)
회전은 부모-자식 노드의 위치를 서로 바꾸는 연산을 말한다.<br>
우회전(Right-Route)은 왼쪽 자식과 부모의 위치를 교환하는 것을 말하고, 좌회전(Left-Route)은 오른쪽 자식과 부모의 위치를 교환하는 것을 말한다.<br>
![레드블랙트리-회전](/assets/img/posts/2021-09-04-red-black-tree-2.png){: width="700" height="150" }<br>
<br>

이진 탐색 트리의 특성과 레드 블랙 트리의 특성을 지키기 위해 회전시 다음과 같이 자식 노드를 처리해줘야 한다.<br>
- 우회전을 할 때는 왼쪽 자식 노드의 오른쪽 자식 노드를 부모 노드의 왼쪽 자식으로 연결한다. 위 그림에서 트리를 우회전하고 나니 노드 6이 노드 8의 왼쪽 자식으로 왔다.
- 좌회전을 할 때는 오른쪽 자식 노드의 왼쪽 자식 노드를 부모 노드의 오른쪽 자식으로 연결한다. 위 그림에서 트리를 좌회전하고 나니 노드 6이 노드 5의 오른쪽 자식으로 됐다.
<br>

### 레드 블랙 트리의 노드 삽입
이진 탐색 트리의 삽입과 같은 원리이지만 레드 블랙 트리의 규칙을 깨지는 않는지 확인해야 한다.<br>
레드 블랙 트리에 새로운 노드가 삽입되면 이 노드를 빨간색으로 칠한 후 양쪽 자식에 NIL 노드를 연결해야 한다.<br>
<br>
그런데 밑의 RBT_InsertNode() 함수를 보면 Nil 노드를 새로 생성해서 NewNode에 연결하지 않고 같은 Nil 노드를 새 노드의 양쪽에 연결한다.<br>
왜냐하면 Nil 노드는 구현의 편의를 위해 도입된 개념이지 실제로 데이터를 담기 위해 사용되는 것이 아니기 때문이다.<br>
데이터를 담지 않는 노드를 각 잎 노드당 두 개씩 할당해서 쓰는 것은 저장 공간 낭비이기 때문에 새 노드를 생성할 때마다 동일한 Nil 노드를 사용한다.<br>

![레드블랙트리-노드삽입1](/assets/img/posts/2021-09-04-red-black-tree-3.png){: width="700" height="150" }<br>
<br><br>

규칙을 다시 보면 다음과 같다.
1. 모든 노드는 빨간색 아니면 검은색이다.
2. 루트 노드는 검은색이다.
3. 잎 노드는 검은색이다.
4. 빨간 노드의 자식들은 모두 검은색이다. 하지만 검은색 노드의 자식이 빨간색일 필요는 없다.
5. 루트 노드에서 모든 잎 노드 사이에 있는 검은색 노드의 수는 모두 동일하다.

여기서 (1)번 규칙은 절대 위배되지 않는다.<br>
(2)번 규칙은 루트 노드를 무조건 검은색으로 칠하면 되니 위배되지 않는다.<br>
(3)번 규칙은 새 노드를 삽입할 때마다 검은색인 NIL 노드를 자식 노드로 덧붙이기 때문에 위배되지 않는다.<br>
새 노드(빨간색)을 삽입할 때는 부모 노드에 연결되어 있던 NIL 노드(검은색)를 떼어내고 그 자리에 연결하기 때문에 (5)번 규칙도 위배되지 않는다.<br>
<br>

(4)번 규칙이 위반되었다는 것은 삽입한 노드와 부모 노드의 색이 모두 빨간색이라는것을 의미한다. 이 상황은 삽입한 노드의 부모 노드의 형제 노드(삼촌)가 어떤 색이냐에 따라 세 가지 경우의 수로 나뉜다.<br>
<br><br>


**1) 삼촌도 빨간색인 경우**<br>
부모 노드와 삼촌 노드를 검은색으로 칠하고, 할아버지 노드를 빨간색으로 칠한다.<br>
![레드블랙트리-노드삽입2](/assets/img/posts/2021-09-04-red-black-tree-4.png){: width="700" height="150" }<br>

할아버지 노드를 빨간색으로 칠함으로 인해 4번 규칙이 다시 위협받기 때문에 할아버지 노드를 새로 삽입한 노드로 간주하고 다시 처음부터 4번 규칙을 위반하는 세 가지 경우를 따져봐야 한다.
<br>

**2) 삼촌이 검은색이며 새로 삽입한 노드가 부모 노드의 오른쪽 자식인 경우**<br>
부모 노드를 왼쪽으로 회전시켜 이 상황을 세 번째 경우의 문제로 바꾼다.<br>
![레드블랙트리-노드삽입3](/assets/img/posts/2021-09-04-red-black-tree-5.png){: width="700" height="150" }<br>

문제가 세 번째 경우로 바뀌면서 새로 삽입한 노드 D가 부모가 되고 부모 노드였던 B가 자식이 되었다.<br>
첫 번째 경우를 처리한 다음에 할아버지 노드를 새로 삽입한 노드로 간주했던 것처럼, 이번에는 부모였던 노드를 새로 삽입한 노드로 간주시키고 세 번째 경우의 문제로 현재 상황을 넘긴다.
<br>

**3) 삼촌이 검은색이며 새로 삽입한 노드가 부모 노드의 왼쪽 자식인 경우**<br>
부모 노드를 검은색, 할아버지 노드를 빨간색으로 칠한 다음 할아버지 노드를 오른쪽으로 회전시킨다.<br>
![레드블랙트리-노드삽입4](/assets/img/posts/2021-09-04-red-black-tree-6.png){: width="700" height="150" }<br>

세 번째 경우를 처리하고 난 다음에는 4번 규칙이 위반되지 않는다. 새로 생성한 노드 B의 부모 D가 검은색이기 때문이다.<br>
D에게 부모가 있었다고 간주해보면, 그 부모 노드의 색이 빨간색이어도, 검은색이어도 여전히 4번 규칙은 위반되지 않는다.<br>
<br>

### 레드 블랙 트리의 노드 삭제
레드 블랙 트리는 기본적으로 이진 탐색 트리의 삭제 연산을 그대로 사용한다.<br>
다만 삭제를 하고 나서 레드 블랙 트리의 규칙이 무너지는데 이를 수습해야 한다는 차이가 있다.<br>
<br>
규칙을 다시 보면 다음과 같다.
1. 모든 노드는 빨간색 아니면 검은색이다.
2. 루트 노드는 검은색이다.
3. 잎 노드는 검은색이다.
4. 빨간 노드의 자식들은 모두 검은색이다. 하지만 검은색 노드의 자식이 빨간색일 필요는 없다.
5. 루트 노드에서 모든 잎 노드 사이에 있는 검은색 노드의 수는 모두 동일하다.

빨간색 노드를 삭제하는 경우에는 신경 쓸 부분이 하나도 없다.<br>
빨간색 노드를 삭제해도 다른 노드의 색이 바뀌지는 않고(1번), 루트 노드나 잎 노드는 원래 검은색이니 지장을 받지 않고(2번, 3번), 빨간색 노드의 부모와 자식들은 원래부터 검은색이니 4번 규칙도 유지된다. 그리고 루트 노드와 잎 노드 사이의 경로 위에 있는 검은색 노드를 셀 때 원래부터 빨간색 노드는 포함되지 않으므로 5번 규칙은 유지된다.<br>
<br>

검은색 노드를 삭제하는 경우엔 삭제한 검은 노드가 놓여 있던 루트-잎 경로의 검은색 노드 수가 다른 루트-잎 경로의 검은색 노드 수보다 하나 더 작은 상태가 되기 때문에 5번 규칙이 위배된다.<br>
삭제한 부모와 삭제한 노드의 자식이 모두 빨간색인 경우에는 4번 규칙도 위배된다.<br>
![레드블랙트리-노드삭제1](/assets/img/posts/2021-09-04-red-black-tree-7.png){: width="700" height="150" }<br>

무너진 4, 5번 규칙을 보완하기 위해서는 삭제된 노드를 대체하는 노드를 검은색으로 칠하면 된다.<br>
아래 그림을 보면 위 그림에서 삭제한 B 노드를 대체한 C 노드에 검은색을 칠함으로써 4, 5번 규칙을 모두 복원하게 된다.<br>
![레드블랙트리-노드삭제2](/assets/img/posts/2021-09-04-red-black-tree-8.png){: width="700" height="150" }<br>

위에서 C 노드가 빨간색이었기 때문에 검은색으로 덧칠하여 문제를 해결했다.<br>
아래와 같이 그럼 C 노드가 검은색인 경우엔 빨간 노드는 검은색 노드를 가져야 한다는 4번 규칙은 위반하지 않고, 모든 루트-잎 경로 사이에 있는 검은색 노드의 수가 동일해야 한다는 5번 규칙은 위반하게 된다.<br>
![레드블랙트리-노드삭제3](/assets/img/posts/2021-09-04-red-black-tree-9.png){: width="700" height="150" }<br>

이 경우에도 대체 노드 C에 검은색을 덧칠한다. 그래서 C 노드는 예외적으로 검은색을 '두 개' 가지게 되고, 규칙 5를 복원하게 된다. 이렇게 검은색을 2개 가지는 노드를 '이중흑색' 노드라고 부튼다.<br>
아래 그림의 오른쪽 트리를 보면 C 노드 위에 검은 정사각형이 있는데 이것은 C 노드가 이중흑색 노드라는 표시이다.<br>
![레드블랙트리-노드삭제4](/assets/img/posts/2021-09-04-red-black-tree-10.png){: width="700" height="150" }<br>


이제 5번 규칙은 복원됐지만 이중흑색 노드로 인해 1번 규칙(모든 노드는 빨간색 아니면 검은색이다)이 위반되었다.<br>
이중흑색 노드를 처리하는 방법은 이중흑색 노드의 형제와 조카들의 상태에 따라 네 가지로 나뉜다.<br>
(아래 예시는 이중흑색 노드가 부모의 왼쪽 자식인 경우이며, 오른쪽 자식인 경우에는 왼쪽/오른쪽만 바꾸면 된다.)<br>
<br>

**1) 형제가 빨간색인 경우**<br>
우선 형제를 검은색, 부모를 빨간색으로 칠한다. 그 다음에는 부모를 기준으로 좌회전 한다. 이렇게 해도 여전히 이중 흑색 노드가 그대로 남아 있지만, 형제 노드는 검은색 노드로 바뀌어있다.<br>
이렇게 형제 노드를 검은색으로 바꿔서 문제의 유형을 2, 3, 4번으로 변경한다.<br>
![레드블랙트리-노드삭제5](/assets/img/posts/2021-09-04-red-black-tree-11.png){: width="700" height="150" }<br>
<br>

**2) 형제가 검은색이고, 형제의 양쪽 자식이 모두 검은색인 경우**<br>
형제 노드만 빨간색으로 칠한 다음, 이중흑색 노드가 갖고 있던 두 개의 검은색 중 하나를 부모 노드에게 넘겨준다.<br>
아래 예시를 보면 형제 노드 D를 빨간색으로 칠하고, C가 갖고 있던 두 개의 검은색 중 하나를 부모 노드인 A에게 넘기면 원래의 이중흑색 노드였던 C에 대한 처리가 끝난다. 부모 노드인 A는 넘겨받은 검은색을 처리해주면 된다. A는 빨간색 노드이기 때문에 형제 노드를 볼 필요 없이 A 노드를 검은색으로 칠해주기만 하면 된다.<br>
![레드블랙트리-노드삭제6](/assets/img/posts/2021-09-04-red-black-tree-12.png){: width="700" height="150" }<br>
<br>

**3) 형제가 검은색이고, 형제의 왼쪽 자식은 빨간색, 오른쪽 자식은 검은색인 경우**<br>
형제 노드를 빨간색으로 칠하고 왼쪽 자식을 검은색으로 칠한 다음, 형제 노드를 기준으로 우회전을 수행한다.<br>
아래 예시를 보면 형제 노드 D를 빨간색, 형제 노드의 왼쪽 자식 노드 E를 검은색으로 칠하고 형제 노드 D를 기준으로 우회전을 수행하면 된다. 그런데 C 노드가 여전히 검은색 두 개를 갖고 있고, 형제가 검은색이며 형제의 오른쪽 자식이 빨간색인 경우로 바뀌었기 때문에 후처리를 수행한다.<br>
![레드블랙트리-노드삭제7](/assets/img/posts/2021-09-04-red-black-tree-13.png){: width="700" height="150" }<br>
<br>

**4) 형제가 검은색이고, 형제의 오른쪽 자식이 빨간색인 경우**<br>
이중흑색 노드의 부모 노드가 갖고 있는 색을 형제 노드에 칠하고 부모 노드와 형제 노드의 오른쪽 자식 노드를 검은색으로 칠하고 부모 노드를 기준으로 좌회전하면 1번 규칙이 복원된다.<br>
아래 예시를 보면 형제 노드 E를 이중흑색 노드 C의 부모 노드인 A의 색인 빨간색으로 칠한다. 그리고 부모 노드 A와 오른쪽 자식 노드인 D를 검은색으로 칠한다. 마지막으로 부모 노드 A를 기준으로 좌회전을 수행한 후 이중흑색 노드 C가 갖고 있던 검은색 중 하나를 루트 노드에 넘긴다.<br>
루트 노드가 이중흑색가 된 경우에는 검은색으로 칠해주기만 하면 되기 때문에 문제가 없다.<br>
![레드블랙트리-노드삭제8](/assets/img/posts/2021-09-04-red-black-tree-14.png){: width="700" height="150" }<br>
<br>

---
## 소스코드

RedBlackTree.h
```c
#ifndef REDBLACKTREE_H
#define REDBLACKTREE_H


#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;

typedef struct tagRBTNode 
{
    struct tagRBTNode* Parent;
    struct tagRBTNode* Left;
    struct tagRBTNode* Right;
    
    /* 노드의 색을 나타내는 Color 필드 */
    /* RED 아니면 BLACK의 값을 저장할 수 있다. */
    enum { RED, BLACK } Color;    

    ElementType Data;

} RBTNode;

void      RBT_DestroyTree( RBTNode* Tree );

RBTNode*  RBT_CreateNode( ElementType NewData );
void      RBT_DestroyNode( RBTNode* Node );

RBTNode*  RBT_SearchNode( RBTNode* Tree, ElementType Target );
RBTNode*  RBT_SearchMinNode( RBTNode* Tree );
void      RBT_InsertNode( RBTNode** Tree, RBTNode *NewNode );
void      RBT_InsertNodeHelper( RBTNode** Tree, RBTNode *NewNode );
RBTNode*  RBT_RemoveNode( RBTNode** Root, ElementType Target );
void      RBT_RebuildAfterInsert( RBTNode** Tree, RBTNode* NewNode );
void      RBT_RebuildAfterRemove( RBTNode** Root, RBTNode* X );

void      RBT_PrintTree( RBTNode* Node, int Depth, int BlackCount );
void      RBT_RotateLeft( RBTNode** Root, RBTNode* Parent );
void      RBT_RotateRight( RBTNode** Root, RBTNode* Parent );

#endif
```
RedBlackTree.c
```c
#include "RedBlackTree.h"

extern RBTNode* Nil;

RBTNode* RBT_CreateNode( ElementType NewData )
{
    RBTNode* NewNode = (RBTNode*)malloc( sizeof(RBTNode) );
    NewNode->Parent  = NULL;
    NewNode->Left    = NULL;
    NewNode->Right   = NULL;
    NewNode->Data    = NewData;
    NewNode->Color   = BLACK;

    return NewNode;
}

void RBT_DestroyNode( RBTNode* Node )
{
    free(Node);
}

void RBT_DestroyTree( RBTNode* Tree )
{
    if ( Tree->Right != Nil )
        RBT_DestroyTree( Tree->Right );

    if ( Tree->Left != Nil )
        RBT_DestroyTree( Tree->Left );

    Tree->Left = Nil;
    Tree->Right = Nil;

    RBT_DestroyNode( Tree );
}

RBTNode*  RBT_SearchNode( RBTNode* Tree, ElementType Target )
{
    if ( Tree == Nil )
        return NULL;
    
    if ( Tree->Data > Target ) 
        return RBT_SearchNode ( Tree->Left, Target );
    else if ( Tree->Data < Target )
        return RBT_SearchNode ( Tree->Right,  Target );
    else 
        return Tree;
}

RBTNode*  RBT_SearchMinNode( RBTNode* Tree )
{
    if ( Tree == Nil )
        return Nil;

    if ( Tree->Left == Nil ) 
        return Tree;
    else
        return RBT_SearchMinNode( Tree->Left );
}

void RBT_InsertNode( RBTNode** Tree, RBTNode* NewNode)
{
    RBT_InsertNodeHelper( Tree, NewNode );

    NewNode->Color = RED;
    NewNode->Left  = Nil;
    NewNode->Right = Nil;

    RBT_RebuildAfterInsert( Tree, NewNode );
}

void RBT_InsertNodeHelper( RBTNode** Tree, RBTNode* NewNode)
{
    if ( (*Tree) == NULL )
        (*Tree) = NewNode;

    if ( (*Tree)->Data < NewNode->Data ) 
    {
        if ( (*Tree)->Right == Nil )
        {
            (*Tree)->Right  = NewNode;
            NewNode->Parent = (*Tree);
        }
        else
            RBT_InsertNodeHelper(&(*Tree)->Right, NewNode);

    } 
    else if ( (*Tree)->Data > NewNode->Data ) 
    {
        if ( (*Tree)->Left == Nil )
        {
            (*Tree)->Left   = NewNode;
            NewNode->Parent = (*Tree);
        }
        else
            RBT_InsertNodeHelper(&(*Tree)->Left, NewNode);
    }
}

void RBT_RotateRight( RBTNode** Root, RBTNode* Parent )
{
    RBTNode* LeftChild =  Parent->Left;

    /* 왼쪽 자식 노드의 오른쪽 자식 노드를 부모 노드의 왼쪽 자식으로 등록 */
    Parent->Left = LeftChild->Right;

    if ( LeftChild->Right != Nil )
        LeftChild->Right->Parent = Parent;

    LeftChild->Parent = Parent->Parent;

    /* 부모가 NIL이라면 이 노드는 Root이다. */
    /* 이 경우에는 왼쪽 자식을 Root 노드로 만들어 회전시킨다. */
    if ( Parent->Parent == NULL )
        (*Root) = LeftChild;
    else
    {
        /* 왼쪽 자식 노드를 부모 노드가 있던 곳(할아버지의 자식 노드)애 위치시킨다. */
        if ( Parent == Parent->Parent->Left )
            Parent->Parent->Left = LeftChild;
        else
            Parent->Parent->Right = LeftChild;
    }

    LeftChild->Right  = Parent;
    Parent->Parent = LeftChild;
}

void RBT_RotateLeft( RBTNode** Root, RBTNode* Parent )
{
    RBTNode* RightChild = Parent->Right;
    
    /* 오른쪽 자식 노드의 왼쪽 자식 노드를 부모 노드의 오른쪽 자식으로 등록 */
    Parent->Right = RightChild->Left;

    if ( RightChild->Left != Nil )
        RightChild->Left->Parent = Parent;

    RightChild->Parent = Parent->Parent;

    /* 부모가 NIL이라면 이 노드는 Root이다. */
    /* 이 경우에는 오른쪽 자식을 Root 노드로 만들어 회전시킨다. */
    if ( Parent->Parent == NULL )
        (*Root) = RightChild;
    else
    {
        /* 오른쪽 자식 노드를 부모 노드가 있던 곳(할아버지의 자식 노드)애 위치시킨다. */
        if ( Parent == Parent->Parent->Left )
            Parent->Parent->Left = RightChild;
        else
            Parent->Parent->Right = RightChild;        
    }

    RightChild->Left   = Parent;
    Parent->Parent = RightChild;
}

void RBT_RebuildAfterInsert( RBTNode** Root, RBTNode* X )
{   
    /* 4번 규칙을 위반하고 있는 동안에는 계속 반복 */
    while ( X != (*Root) && X->Parent->Color == RED ) 
    {
        /* 부모 노드가 할아버지 노드의 왼쪽 자식인 경우 */
        if ( X->Parent == X->Parent->Parent->Left ) 
        {
            RBTNode* Uncle = X->Parent->Parent->Right;
            /* 삼촌이 빨간색인 경우 */
            if ( Uncle->Color == RED ) 
            {
                X->Parent->Color         = BLACK;
                Uncle->Color             = BLACK;
                X->Parent->Parent->Color = RED;

                X = X->Parent->Parent;
            }
            else 
            {
                /* 삼촌이 검은색이고 X가 오른쪽 자식일 때 */
                if ( X == X->Parent->Right ) 
                {
                    X = X->Parent;
                    RBT_RotateLeft( Root, X );
                }
                
                X->Parent->Color         = BLACK;
                X->Parent->Parent->Color = RED;

                RBT_RotateRight( Root, X->Parent->Parent );
            }
        }
        else 
        {
            RBTNode* Uncle = X->Parent->Parent->Left;
            if ( Uncle->Color == RED ) 
            {
                X->Parent->Color         = BLACK;
                Uncle->Color             = BLACK;
                X->Parent->Parent->Color = RED;

                X = X->Parent->Parent;
            }
            else 
            {
                if ( X == X->Parent->Left ) 
                {
                    X = X->Parent;
                    RBT_RotateRight( Root, X );
                }
                
                X->Parent->Color         = BLACK;
                X->Parent->Parent->Color = RED;
                RBT_RotateLeft( Root, X->Parent->Parent );
            }
        }
    }
    /* 루트 노드는 반드시 검은색 */
    (*Root)->Color = BLACK;
}

RBTNode* RBT_RemoveNode( RBTNode** Root, ElementType Data )
{
    RBTNode* Removed   = NULL;
    RBTNode* Successor = NULL;
    RBTNode* Target    = RBT_SearchNode( (*Root), Data );

    if ( Target == NULL )
        return NULL;

    if ( Target->Left == Nil || Target->Right == Nil )
    {
        Removed = Target;
    }
    else 
    {
        Removed = RBT_SearchMinNode( Target->Right );
        Target->Data = Removed->Data;
    }

    if ( Removed->Left != Nil )
        Successor = Removed->Left;
    else
        Successor = Removed->Right;


    Successor->Parent = Removed->Parent;

    if ( Removed->Parent == NULL )
        (*Root) = Successor;
    else
    {
        if ( Removed == Removed->Parent->Left )
            Removed->Parent->Left = Successor;
        else
            Removed->Parent->Right = Successor;
    }
    /* 삭제한 노드가 검은색인 경우 대체 노드를 RBT_RebuildAfterRemove() 함수에 매개변수로 넘겨 호출 */
    if ( Removed->Color == BLACK ) 
        RBT_RebuildAfterRemove( Root, Successor );    

    return Removed;
}

void RBT_RebuildAfterRemove( RBTNode** Root, RBTNode* Successor )
{
    RBTNode* Sibling = NULL;

    /* 1 루트 노드이거나 빨간색 노드한테 검은색이 넘어가면 루프 종료 */
    while ( Successor->Parent != NULL && Successor->Color == BLACK)
    {
        if ( Successor == Successor->Parent->Left )
        {
            Sibling = Successor->Parent->Right;

            /* 형제가 빨간색인 경우 */
            if ( Sibling->Color == RED )
            {
                Sibling->Color = BLACK;
                Successor->Parent->Color = RED;
                RBT_RotateLeft( Root, Successor->Parent );
            }
            else /* 형제가 검은색이며 */
            {
                if ( Sibling->Left->Color == BLACK && 
                        Sibling->Right->Color == BLACK ) /* 2. 양쪽 자식이 모두 검은색인 경우 */
                {
                    Sibling->Color = RED;
                    Successor      = Successor->Parent;
                }
                else 
                {
                    if ( Sibling->Left->Color == RED ) /* 3. 왼쪽 자식이 빨간색인 경우 */
                    {
                        Sibling->Left->Color = BLACK;
                        Sibling->Color       = RED;

                        RBT_RotateRight( Root, Sibling );
                        Sibling = Successor->Parent->Right;
                    }
                    /* 4. 오른쪽 자식이 빨간색인 경우 */
                    Sibling->Color           = Successor->Parent->Color;
                    Successor->Parent->Color = BLACK;
                    Sibling->Right->Color    = BLACK;
                    RBT_RotateLeft( Root, Successor->Parent );
                    Successor = (*Root);
                }
            }            
        } 
        else
        {
            Sibling = Successor->Parent->Left;

            if ( Sibling->Color == RED )
            {
                Sibling->Color           = BLACK;
                Successor->Parent->Color = RED;
                RBT_RotateRight( Root, Successor->Parent );
            }
            else
            {
                if ( Sibling->Right->Color == BLACK && 
                        Sibling->Left->Color  == BLACK )
                {
                    Sibling->Color = RED;
                    Successor      = Successor->Parent;
                }
                else 
                {
                    if ( Sibling->Right->Color == RED )
                    {
                        Sibling->Right->Color = BLACK;
                        Sibling->Color        = RED;

                        RBT_RotateLeft( Root, Sibling );
                        Sibling = Successor->Parent->Left;
                    }

                    Sibling->Color           = Successor->Parent->Color;
                    Successor->Parent->Color = BLACK;
                    Sibling->Left->Color     = BLACK;
                    RBT_RotateRight( Root, Successor->Parent );
                    Successor = (*Root);
                }
            }
        }
    }

    Successor->Color = BLACK;
}

void RBT_PrintTree( RBTNode* Node, int Depth, int BlackCount )
{
    int   i = 0;
    char c = 'X';
    int  v = -1;
    char cnt[100];

    if ( Node == NULL || Node == Nil)
        return;

    if ( Node->Color == BLACK )
        BlackCount++;

    if ( Node->Parent != NULL ) 
    {
        v = Node->Parent->Data;

        if ( Node->Parent->Left == Node )
            c = 'L';            
        else
            c = 'R';
    }

    if ( Node->Left == Nil && Node->Right == Nil )
        sprintf(cnt, "--------- %d", BlackCount );
    else
        sprintf(cnt, "");

    for ( i=0; i<Depth; i++)
        printf("  ");

    printf( "%d %s [%c,%d] %s\n", Node->Data, 
            (Node->Color == RED)?"RED":"BLACK", c, v, cnt);
    
    RBT_PrintTree( Node->Left, Depth+1, BlackCount);
    RBT_PrintTree( Node->Right, Depth+1, BlackCount );
}
```
Test_RedBlackTree.c
```c
#include "RedBlackTree.h"

RBTNode* Nil;

int main( void )
{
    RBTNode* Tree = NULL;
    RBTNode* Node = NULL;
    
    Nil           = RBT_CreateNode(0);
    Nil->Color    = BLACK;    

    while ( 1 )
    {
        int  cmd   = 0;
        int  param = 0;
        char buffer[10];

        printf("Enter command number :\n");
        printf("(1) Create a node, (2) Remove a node, (3) Search a Node\n");
        printf("(4) Display Tree (5) quit\n");
        printf("command number:");

        fgets(buffer, sizeof(buffer)-1, stdin);
        sscanf(buffer, "%d", &cmd );

        if ( cmd < 1 || cmd > 5 )
        {
            printf("Invalid command number.\n");
            continue;
        }
        else if ( cmd == 4 )
        {
            RBT_PrintTree( Tree, 0, 0 );
            printf( "\n");
            continue;
        }
        else if ( cmd == 5 )
            break;

        printf("Enter parameter (1~200) :\n");

        fgets(buffer, sizeof(buffer)-1, stdin);
        sscanf(buffer, "%d", &param );

        if ( param < 1 || param > 200 )
        {
            printf("Invalid parameter.%d\n", param);
            continue;
        }

        switch ( cmd )
        {
        case 1:
            RBT_InsertNode( &Tree, RBT_CreateNode(param) );            
            break;
        case 2:
            Node = RBT_RemoveNode( &Tree, param);
                
            if ( Node == NULL ) 
                printf("Not found node to delete:%d\n", param);
            else
                RBT_DestroyNode( Node );

            break;
        case 3:
            Node = RBT_SearchNode(Tree, param);
                
            if ( Node == NULL ) 
                printf("Not found node:%d\n", param);            
            else
                printf("Found Node: %d(Color:%s)\n", 
                        Node->Data, (Node->Color==RED)?"RED":"BLACK");            
            break;
        }

        printf("\n");

    }

    RBT_DestroyTree( Tree );
    return 0;
}
```
실행결과
```
Enter Command number :
(1) Create a node, (2) Remove a node, (3) Search a Node
(4) Display Tree (5) quit
command number:1
Enter parameter (1~200) :
95

Enter Command number :
(1) Create a node, (2) Remove a node, (3) Search a Node
(4) Display Tree (5) quit
command number:4
87 BLACK [X,-1]
    13 RED [L,87]
        12 BLACK [L,13] --------- 2
        15 BLACK [R,13]
            14 RED [L,15] --------- 2
            32 RED [R,15] --------- 2
    123 BLACK [R,87]
        95 RED [L,123] --------- 2
        174 RED [R,123] --------- 2

Enter Command number :
(1) Create a node, (2) Remove a node, (3) Search a Node
(4) Display Tree (5) quit
command number:
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>