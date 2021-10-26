---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 15. 백트래킹"
author: 임가람
date: "2021-09-26 17:20:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 백트래킹(Backtracking)
여러 후보해 중에서 특정 조건을 충족시키는 모든 해를 찾는 알고리즘, 백트래킹의 문제들은 해가 하나 이상 존재한다.<br>
예를 들어 탈출로가 두 개인 미로 문제는 해가 두 개라고 할 수 있다. 이 두 해를 일걸어 `모든 해`라고 한다. 미로의 한 지점에서 탈출구로 향하는 경로 속에는 여러 길목이 있는데 이들 각 길목에서 어느 쪽으로 갈 지의 물음(부분 문제)에 대한 답을 `부분 해`라고 하며, 이 과정을 반복하면서 완성한 것이 탈출로(해)이다. 즉 해가 될 수 있는 가능성을 가진 부분해의 조합을 두고 `후보 해`라고 한다.<br>
다음 그림과 같은 미로에서 탈출로를 찾아야 한다고 하자. 미로의 S 지점에서 탈출구 G 지점으로 이동해야 한다.<br>
![백트래킹 1](/assets/img/posts/2021-09-26-backtracking-1.png){: width="700" height="150" }

길을 가다가 갈래길목을 만나면 어느 쪽으로 갈 것인가 선택해야 한다. 이 선택의 경우의 수를 트리로 나타내면 다음과 같다.<br>
![백트래킹 2](/assets/img/posts/2021-09-26-backtracking-2.png){: width="700" height="150" }

출발점 S를 지나 만날 수 있는 새로운 길은 1, 2번이다. 1번 길에는 더 이상 새로운 길이 나오지 않고 2번 길에는 3, 4번으로 갈 수 있는 길목이 나온다. 4번 길을 가면 막다른 곳이 나오고 3번 길로 가면 5, 6번 길목을 만난다. 5번 길을 가면 막다른 곳이 나오고 6번 길으로 가면 탈출구 G를 만날 수 있다.<br>
미로 탐색을 위한 탐색 규칙은 다음과 같다.<br>
1. 갈래 길이 나오면 무조건 왼쪽 길부터 들어간다.
2. 막다른 곳이 나오면 갈래 길목으로 되돌아 나와 그 다음 길을 시도한다.
3. 목표 지점에 도달하거나 모든 경로를 다 탐색할 때까지 1, 2번 과정을 반복한다.

이 규칙에 따라 미로를 탐색하면 다음 그림과 같은 과정을 거쳐 S에서 G에 이르는 탈출로를 얻을 수 있다.<br>
![백트래킹 3](/assets/img/posts/2021-09-26-backtracking-3.png){: width="700" height="150" }

이 과정은 깊이 우선 탐색 알고리즘과 비슷하다. 깊이 우선 탐색이 모든 노드를 방문하는 것이 목적이고, 백트래킹은 해를 찾는 것이 목적인것이 다른점이다.<br>
백트래킹은 해를 찾는 비용을 줄이기 위해 최대한 방문할 노드의 수를 최소화하는 것이 중요하다.<br>
![백트래킹 4](/assets/img/posts/2021-09-26-backtracking-4.png){: width="700" height="150" }

백트래킹이 다루는 문제들은 다양한 후보해를 갖고 있는데, 이 후보해들은 부분해로 이루어져 있어서 방금 전에 풀었던 미로 문제와 같이 트리 형태로 표현할 수 있다. 이런 후보해 속에서 해를 찾아가는 과정은 다음과 같다.<br>
1. 해를 찾아가는 과정은 '루트'에서부터 출발한다.(루트도 하나의 부분해이다.)
2. 현재 위치한 부분해에서 선택이 가능한 다음 부분해의 목록을 얻는다.
3. 2번에서 얻은 목록의 부분해들을 하나씩 방문한다.
4. 방문한 부분해가 해가 요구하는 조건을 만족시키면 그 자리에서 2, 3번 과정을 수행하고, 그렇지 않으면 그 이전 부분해로 돌아 나와 다른 부분해를 시도한다.
5. 최종해를 얻을 때까지, 또는 모든 경우의 수를 확인해도 해가 없음을 확인했을 때까지 2~4번 과정을 반복한다.

<br>

---
## 미로 탈출로 찾기
### 트리 대신 재귀 호출로 구현하는 백트래킹

노드 순회가 '노드 이동 → 자식 노드 목록 확인 → 각 자식 노드 탐색'의 반복으로 이루어지는것 처럼 백트래킹에서 해를 구하는 과정은 '부분해 계산 → 다음 부분 후보해 목록 확인 → 각 부분 후보해 계산'의 반복으로 이루어진다. 그리고 부분해를 구하기 위해 이루어지는 반복 과정은 `재귀 호출`로 구현할 수 있다.<br>
백트래칭을 처리해야 하는 경우는 현재 시도 중인 부분 후보해가 해가 될 수 있는 조건을 만족시키지 못할 때와 최종해를 확보한 경우(트리에서는 해의 조건을 만족시키는 잎 노드) 두 가지이다.<br>
상위 부분해가 호출한 후보 부분해의 재귀 함수에서 백트래킹 조건을 만나면, 함수를 반환하여 사우이 부분해를 시도 중인 함수로 제어를 넘겨 백트래킹이 이루어진다. 제어를 돌려받은 상위 부분해는 나머지 부분 후보해들을 차례대로 시도하고, 모든 부분 후보해에 대해 시도를 마쳤으면 여기에서도 함수를 반환하여 자신을 호출한 더 상위 부분해로 돌아가여 반복한다.<br>
<br>

### 미로 탈출 알고리즘 구현하기

우선 미로를 기호로 표현하면 다음과 같다.<br>
![백트래킹 5](/assets/img/posts/2021-09-26-backtracking-5.png){: width="700" height="150" }

미로 탈출 알고리즘은 다음의 과정으로 진행된다.<br>
1. 시작점 S를 현재 위치로 지정하고, 이동 방향을 북으로 설정한다.
2. 현 위치에서 가고자 하는 방향에 대해 이동 가능한지를 확인한다. 벽과 지나왔던 길은 이동 가능한 길이 아니다.
3. 2번 과정에서 현재 방향에 대해 이동 가능한 방향임이 확인되면, 그곳으로 이동한다. 이동이 불가능한 방향으로 판정되면 방향(북→남→동→서 순)을 바꾸어서 다시 2번을 실행한다. 현 위치에서 북→남→동→서 어떤 방향으로도 이동할 수 없음이 확인되면 이전 위치로 돌아간다.
4. 출구를 찾거나 미로 내의 모든 길을 방문할 때까지 2, 3번 과정을 반복한다.

미로 예제는 데이터 파일 2개와 소스 코드 3개로 이루어져있다.<br>
[MazeSample.dat 파일 다운로드](/assets/file/MazeSample.dat){:target="_blank"} <br>
[MazeSample2.dat 파일 다운로드](/assets/file/MazeSample2.dat){:target="_blank"}

MazeSolver.h
```c
#ifndef MAZESOLVER_H
#define MAZESOLVER_H

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_BUFFER 1024
#define INIT_VALUE -1

#define START    'S'    /* 시작점 */
#define GOAL     'G'    /* 탈출구 */
#define WAY      ' '    /* 길 */
#define WALL     '#'    /* 벽 */
#define MARKED   '+'    /* 탈출구로 향하는 길 표식 */


enum DIRECTION { NORTH, SOUTH, EAST, WEST };
enum RESULT    { FAIL, SUCCEED }; 
/* 미로 안의 위치를 나타내기 위한 구조체 */
typedef struct tagPosition
{
    int X;
    int Y;
} Position;
/* 미로를 표현하는 구조체 */
typedef struct tagMazeInfo
{
    int ColumnSize;     /* 너비 */
    int RowSIze;        /* 높이 */

    char** Data;        /* 동적으로 할당한 2차원 배열을 담기 위한 2차원 포인터 */
} MazeInfo;

int Solve( MazeInfo* Maze );
int MoveTo( MazeInfo* Maze, Position* Current, int Direction );
int GetNextStep( MazeInfo* Maze, Position* Current, int Direction, Position* Next );
int GetMaze( char* FilePath, MazeInfo* Maze );

#endif
```
MazeSolver.c
```c
#include "MazeSolver.h"

int Solve( MazeInfo* Maze )
{
    int i=0;
    int j=0;
    int StartFound = FAIL;
    int Result = FAIL;

    Position Start;

    /* 시작점 S를 찾는다. */
    for ( i=0; i<Maze->RowSIze; i++ )
    {
        for ( j=0; j<Maze->ColumnSize; j++ )
        {
            if ( Maze->Data[i][j] == START )
            {
                Start.X = j; // Column
                Start.Y = i; // Row
                StartFound = SUCCEED;
                break;
            }
        }
    }

    if ( StartFound == FAIL )
        return FAIL;    

    /* 북, 남, 동, 서 순으로 MoveTo()를 시도한다. */
    /* 어느 방향에서도 FAIL을 반환한다면 탈출로는 존재하지 않는다. */
    if ( MoveTo ( Maze, &Start, NORTH ) ) 
        Result = SUCCEED;
    else if ( MoveTo ( Maze, &Start, SOUTH ) ) 
        Result = SUCCEED;  
    else if ( MoveTo ( Maze, &Start, EAST  ) ) 
        Result = SUCCEED;  
    else if ( MoveTo ( Maze, &Start, WEST  ) ) 
        Result = SUCCEED;

    Maze->Data[Start.Y][Start.X] = START;

    return Result;
}

/* 출구를 찾거나 모든 후보해를 방문할 때까지 호출을 반복 */
int MoveTo( MazeInfo* Maze, Position* Current, int Direction )
{
    int i=0;

    /* 코딩의 편의를 위해 네 방향을 배열에 담는다. */
    int Dirs[] = { NORTH, SOUTH, WEST, EAST };

    Position Next;

    if ( Maze->Data[Current->Y][Current->X] == GOAL )     
        return SUCCEED;

    /* 현재 위치를 지나왔음 (MARKED로 표시한다.) */
    Maze->Data[Current->Y][Current->X] = MARKED;

    for ( i=0; i<4; i++ )
    {
        if ( GetNextStep( Maze, Current, Dirs[i], &Next ) == FAIL )
            continue;

        if ( MoveTo ( Maze, &Next, NORTH ) == SUCCEED )  
            return SUCCEED;
    }
    
    /* 모든 방향에 대해 실패했다. 이곳은 답이 아니니 원래대로 길(WAY)로 표시하고 이전 위치로 백트래킹한다. */
    Maze->Data[Current->Y][Current->X] = WAY;

    return FAIL;
}

/* 왔던 곳, 벽 등을 피해 다음으로 이동할 곳을 찾음 */
int GetNextStep( MazeInfo* Maze, Position* Current, int Direction, Position* Next )
{
    /* Current의 다음 위치가 미로의 경계를 넘어서면 FAIL을 반환한다. */
    switch ( Direction )
    {
    case NORTH:
        Next->X = Current->X;
        Next->Y = Current->Y - 1;

        if ( Next->Y == -1 ) return FAIL;

        break;
    case SOUTH:
        Next->X = Current->X;
        Next->Y = Current->Y + 1;

        if ( Next->Y == Maze->RowSIze ) return FAIL;

        break;
    case WEST:
        Next->X = Current->X - 1;
        Next->Y = Current->Y;

        if ( Next->X == -1 ) return FAIL;
        
        break;
    case EAST:
        Next->X = Current->X + 1;
        Next->Y = Current->Y;        

        if ( Next->X == Maze->ColumnSize ) return FAIL;
        
        break;
    }    
    /* 다음 갈 곳이 벽이나 지나온 길이면 FAIL을 반환한다. */
    if ( Maze->Data[Next->Y][Next->X] == WALL )     return FAIL;
    if ( Maze->Data[Next->Y][Next->X] == MARKED )   return FAIL;

    return SUCCEED;
}

int GetMaze( char* FilePath, MazeInfo* Maze )
{
    int i=0;
    int j=0;
    int RowSize    = 0;
    int ColumnSize = INIT_VALUE;

    FILE* fp;
    char  buffer[MAX_BUFFER];
    
    if ( (fp = fopen( FilePath, "r"))  == NULL )
    {
        printf("Cannot open file:%s\n", FilePath);
        return FAIL;
    }

    while ( fgets( buffer, MAX_BUFFER, fp ) != NULL )
    {
        RowSize++;

        if ( ColumnSize == INIT_VALUE )
        {
            ColumnSize = strlen( buffer ) - 1;
        }
        else if ( ColumnSize != strlen( buffer ) - 1 )
        {
            printf("Maze data in file:%s is not valid. %d\n", 
                FilePath, strlen( buffer ));
            fclose( fp );
            return FAIL;
        }
    }

    Maze->RowSIze    = RowSize;
    Maze->ColumnSize = ColumnSize;
    Maze->Data = (char**)malloc(sizeof(char*) * RowSize );

    for ( i=0; i<RowSize; i++ )
        Maze->Data[i] = (char*)malloc(sizeof(char) * ColumnSize);

    rewind( fp );

    
    for ( i=0; i<RowSize; i++ )
    {
        fgets( buffer, MAX_BUFFER, fp );

        for ( j=0; j<ColumnSize; j++ )
        {
            Maze->Data[i][j] = buffer[j];
        }
    }

    fclose( fp );
    return SUCCEED;
}
```
Test_MazeSolver.c
```c
#include <stdio.h>
#include "MazeSolver.h"

int main(int argc, char* argv[])
{
    int i = 0;
    int j = 0;

    MazeInfo Maze;

    if ( argc < 2 )
    {
        printf("Usage: MazeSolver <MazeFile>\n");
        return 0;
    }

    if ( GetMaze( argv[1], &Maze ) == FAIL )
        return 0;

    if ( Solve( &Maze ) == FAIL )
        return 0;
    
    for ( i=0; i<Maze.RowSIze; i++ )
    {
        for ( j=0; j<Maze.ColumnSize; j++ )
        {
            printf("%c", Maze.Data[i][j] );
        }
        printf("\n");
    }
    
    return 0;
}
```

<br>

---
### 8개의 퀸

체스의 퀸은 아래 그림처럼 8가지 방향으로 움직이며 공격을 할 수 있고, 이동 범위도 체스판 전체에 이르기 떄문에 게임의 승부를 가르는 데 아주 중요한 역할을 한다.<br>
![8개의퀸 1](/assets/img/posts/2021-09-26-backtracking-6.png){: width="700" height="150" }

이러한 퀸의 속성을 이용해 막스 베젤(Max Bezzel)이라는 체스 선수가 '8개의 퀸' 이라는 퍼즐을 만들었다.<br>
'8개의 퀸' 문제는 퀸 8개를 8 $\times$ 8 크기의 체스판 안에 서로를 공격할 수 없게 배치하는 모든 경우를 구하는 문제이다.<br>
예를 들어 첫 번쨰퀸을 [3, 4] 위치에 놓으면 [5, 6]에는 두 번째 퀸을 놓을 수 없다.<br>
![8개의퀸 2](/assets/img/posts/2021-09-26-backtracking-7.png){: width="700" height="150" }

아래 그림처럼 두 퀸이 [3, 4], [5, 5]에 놓여 있으면 서로 공격할 수 없다.<br>
![8개의퀸 3](/assets/img/posts/2021-09-26-backtracking-8.png){: width="700" height="150" }

체크판의 칸이 모두 64개이고, 기 중에서 8개를 골라 퀸을 배치할 수 있으므로 후보해의 수는 총 $_{64}C_{8}=\dfrac{64!}{8!(64-8)!}=4,426,165,368$개 이다. 이 중에서 실제 해는 92개 뿐이다.<br>
8개의 퀸 문제는 다른 크기의 체스판으로도 문제의 축소/확장이 가능하다. 4 $\times$ 4 이상의 크기라면 이 문제를 적용할 수 있어서 `N개의 퀸 문제`라고 부르기도 한다.<br>
<br>

### 8개의 퀸이 만드는 해공간과 백트래킹
아래의 예시는 8개의 퀸 문제의 해공간을 어떻게 구축하는지 보기 위해 4 $\times$ 4 체스판을 활용한다.<br>
우선 첫 번쨰 퀸을 놓을 수 있는 위치는 [1, 1], [1, 2], [1, 3], [1, 4] 네 가지 경우이다. 네 가지 첫 번째 말의 위치는 최종해의 부분 후보해이다.<br>
![8개의퀸 4](/assets/img/posts/2021-09-26-backtracking-9.png){: width="700" height="150" }

[1, 1] 부분해부터 보면, [1, 1]과 [2, 1]은 서로 수직 방향으로 위험하고, [2, 2]는 대각선 방향으로 서로 공격이 가능하다. [1, 1]에 대해 [2, 3]과 [2, 4]는 공격이 불가능하므로 부분해로 수용한다.<br>
![8개의퀸 5](/assets/img/posts/2021-09-26-backtracking-10.png){: width="700" height="150" }

하지만 [2, 3]은 수용할 수 있는 세 번째 부분해가 없고, [2, 4]는 유일하게 수용 가능한 [3, 2]의 네 번째 부분해가 없으므로 둘 다 폐기해야 한다. 결국 [1, 1] 부분해도 폐기하고 첫 번째 부분해 선택으로 백트래킹 해야한다.<br>
![8개의퀸 6](/assets/img/posts/2021-09-26-backtracking-11.png){: width="700" height="150" }

다시 처음으로 돌아와서 [1, 2]를 첫 번째 부분해로 선택한다, [1, 2]에 이은 두 번째 부분해 중 [2, 1], [2, 2], [2, 3]은 모두 [1, 2]를 위협하는 위치이기 때문에 버린다. [2, 4]만 [1, 2]를 위협하지 않기 떄문에 이를 두 번쨰 부분해로 채택한다.<br>
[2, 4]에 이은 부분해를 보면 [3, 1]를 제외한 나머지가 모두 [1, 2]나 [2, 4]를 위협하는 위치이므로 [3, 1]을 세 번째 부분해로 채택한다.<br>
[3, 1]의 후보해 네 개 중 [4, 3]을 빼고는 모두 수용할 수 없으므로 [4, 3]을 네 번째 부분해로 채택한다.<br>
![8개의퀸 7](/assets/img/posts/2021-09-26-backtracking-12.png){: width="700" height="150" }

이렇게 해서 [1, 2]-[2, 4]-[3, 1]-[4, 3]으로 이어지는 첫 번째 해를 얻었다.<br>
<br>

### 8개의 퀸 알고리즘 구현하기
IsThreatened() 함수 에서는 새로 놓은 퀸이 기존의 퀸들을 위협하는지 확인한다. 위협 여부를 판단하는 방법은 다음과 같다.<br>
- 수직 방향의 위협을 알아내기: 이전 단계에서 놓은 퀸들이 위치한 열 중에 새롭게 놓은 퀸 위치의 열과 같은 것이 있는지 확인한다. 있다면 새롭게 놓인 퀸은 다른 퀸을 수직으로 위협하는 것이다.
- 수평 방향 위협 알아내기: 이전 단계에서 놓은 퀸들이 위치한 행 중에 새롭게 놓은 퀸이 위치한 행과 같은 것이 있는지 확인한다. 있다면 새롭게 놓인 퀸은 다른 퀸을 수평으로 위협하는 것이다.
- 대각선 방향 위협 알아내기: 두 퀸의 위치가 다음의 등식을 만족하면 이 둘은 서로를 대각선으로 위협하는 것이다.<br>|행1-행2|=|열1-열2|<br>예를들어 아래와 같이 퀸[3, 4]와 퀸[6, 7]은 |3-6|=|4-7|을 만족하므로 서로를 대각선 방향으로 위협한다.
![8개의퀸 8](/assets/img/posts/2021-09-26-backtracking-13.png){: width="700" height="150" }

아래 예제 코드는 새로운 행을 매개 변수로 입력받아 실행되기 때문에 IsThreatened() 함수에 수평 방향 위협은 따로 체크하지 않는다.<br>
<br>
NQueens.h
```c
#ifndef NQUEENS_H
#define NQUEENS_H

void PrintSolution( int Columns[], int NumberOfQueens );
int  IsThreatened ( int Columns[], int NewRow );
void FindSolutionForQueen( int Columns[], int Row, 
                           int NumberOfQueens, int* SolutionCount );

#endif
```
NQueens.c
```c
#include "NQueens.h"

#include <stdio.h>
#include <stdlib.h>
/* N개의 퀸의 해를 찾는 함수 */
/* Columns: 부분해를 담고 있는 배열 */
/* Rows: 새 퀸이 놓인 행 번호 */
/* NumberOfQueens: 체스판의 크기 */
/* SolutionCount: 해가 몇 개인지 세기 위한 변수 */
void FindSolutionForQueen(int Columns[], int Row, 
                          int NumberOfQueens, int* SolutionCount) 
{
    if ( IsThreatened( Columns, Row ) )
        return;
    
    if ( Row == NumberOfQueens-1 )
    {
        printf("Solution #%d : \n", ++(*SolutionCount));
        PrintSolution( Columns, NumberOfQueens );
    }
    else /*  */
    {
        int i;

        for ( i=0; i<NumberOfQueens; i++ )
        {
            Columns[Row+1] = i;
            FindSolutionForQueen ( Columns, Row+1, 
                                   NumberOfQueens, SolutionCount );
        }
    }
}
/* 새로 놓은 퀸이 기존의 퀸들을 위협하는지 확인하는 함수 */
/* Columns: 부분해를 담고 있는 배열 */
/* NewRow: 새로 놓은 퀸이 놓인 행 번호 */
int IsThreatened ( int Columns[], int NewRow )
{
    int CurrentRow = 0;
    int Threatened = 0;

    while ( CurrentRow < NewRow )
    { 
        if ( Columns[NewRow] == Columns[CurrentRow] /* 수직 위협 체크 */
             || abs (Columns[NewRow] - Columns[CurrentRow])  /* 대각선 위협 체크 */
               == abs(NewRow - CurrentRow))        
        {
            Threatened = 1;
            break;
        }

       CurrentRow++;
    }

    return Threatened;
}

void PrintSolution( int Columns[], int NumberOfQueens ) 
{
    int i=0;
    int j=0;

    for ( i=0; i<NumberOfQueens; i++ )
    {
        for ( j=0; j<NumberOfQueens; j++ )
        {
            if ( Columns[i] == j )
                printf("Q");
            else
                printf(".");
        }

        printf("\n");
    }
    printf("\n");
}
```
Test_NQueens.c
```c
#include <stdio.h>
#include <stdlib.h>
#include "NQueens.h"

int main(int argc, char* argv[]) 
{
    int i              = 0;
    int NumberOfQueens = 0; 
    int SolutionCount  = 0;
    int* Columns;

    if ( argc < 2 )
    {
        printf("Usage: %s <Number Of Queens>", argv[0]);
        return 1;
    }
    
    NumberOfQueens = atoi(argv[1]); 
    Columns = (int*)calloc( NumberOfQueens, sizeof(int) );
        
    for ( i=0; i<NumberOfQueens; i++ )
    {
        Columns[0] = i;
        FindSolutionForQueen(Columns, 0, NumberOfQueens, &SolutionCount);
    }    

    free ( Columns );

    return 0;
}
```
실행 결과
```
>NQueens 8

Solution #1 :
Q.......
....Q...
.......Q
.....Q..
..Q.....
......Q.
.Q......
...Q....


Solution #2 :
Q.......
.....Q..
.......Q
..Q.....
......Q.
...Q....
.Q......
....Q...

...

Solution #92 :
.......Q
...Q....
Q.......
..Q.....
.....Q..
.Q......
......Q.
....Q...
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>