---
layout: book
---

선택정렬
---

<br>

### 정렬 방법
 
 - 맨 앞의 숫자를 선택한다.
 - 리스트에서 가장 작은 숫자를 구한다.
 - 맨 앞의 숫자와 가장 작은 숫자의 위치를 바꾼다.
 - 마지막까지 반복한다.

<br><br>

### 예시

- 초기상태
  - 9 6 7 3 5
- 1회전
  - 9를 선택하고 나머지 6, 7, 3, 5중 가장 작은 수와 위치 바꾸기
  - [3] 6 7 [9] 5
- 2회전
  - 6을 선택하고 나머지 7, 9, 5중 가장 작은 수와 위치 바꾸기
  - 3 [5] 7 9 [6]
- 3회전
  - 7을 선택하고 나머지 9, 6중 가장 작은 수와 위치 바꾸기
  - 3 5 [6] 9 [7]
- 4회전
  - 9를 선택하고 나머지 7과 비교하여 작은 수를 앞으로 위치 바꾸기
  - 3 5 6 [7] [9]
- 결과
  - 3 5 6 7 9
 

<br><br>

### 소스코드

```java
import java.util.Arrays;

public class Solve {

    public static void main(String[] args) {

        int arr[] = {9, 6, 7, 3, 5}; // 정렬할 배열
        int minIdx; // 가장 작은 숫자의 위치를 저장해놓는 변수
        int temp; // 선택한 숫자와 가장 작은 숫자의 위치를 바꿀 때 사용하는 변수

        System.out.println("리스트 : " + Arrays.toString(arr));

        for (int i = 0; i < arr.length; i++) {
            minIdx = i; // 현재 위치를 가장 작은 숫자로 선택
            for (int j = i + 1; j < arr.length; j++) { // 현재 위치의 다음 숫자부터 배열의 끝까지 반복
                if (arr[j] < arr[minIdx]) { // 가장 작은 숫자의 위치를 minIdx에 저장
                    minIdx = j;
                }
            }

            // 현재 위치와 가장 작은 숫자의 위치를 바꿈
            temp = arr[minIdx];
            arr[minIdx] = arr[i];
            arr[i] = temp;
            System.out.println(i + 1 + "회전 : " + Arrays.toString(arr));
        }
    }
}


/* 출력
리스트 : [9, 6, 7, 3, 5]
1회전 : [3, 6, 7, 9, 5]
2회전 : [3, 5, 7, 9, 6]
3회전 : [3, 5, 6, 9, 7]
4회전 : [3, 5, 6, 7, 9]
5회전 : [3, 5, 6, 7, 9]
*/
```
