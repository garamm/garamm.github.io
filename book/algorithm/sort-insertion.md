---
layout: book
---

삽입정렬
---


<br>

### 정렬 방법
- 두번째 숫자를 선택한 후 첫번째 숫자와 비교하여 알맞은 곳에 삽입한다.
- 세번째 숫자를 선택한 후 첫번째, 두번째 숫자와 비교하여 알맞은 곳에 삽입한다.
- ...
- n번째 숫자를 선택한 후 첫번째, ... , n-1번째 숫자와 비교하여 알맞은 곳에 삽입한다.

<br><br>

### 예시

- 9 6 7 3 5 (초기상태)
- 6 [9] 7 3 5
- 6 7 [9] 3 5
- 3 6 7 [9] 5
- 3 5 6 7 [9]

<br><br>

### 소스코드

```java
import java.util.Arrays;

public class Solve {

    public static void main(String[] args) {

        int arr[] = {9, 6, 7, 3, 5}; // 정렬할 배열

        System.out.println("리스트 : " + Arrays.toString(arr));
        
        for(int i = 1 ; i < arr.length ; i++) {
            int temp = arr[i];
            int aux = i - 1;
            while( (aux >= 0) && ( arr[aux] > temp ) ) {
                arr[aux+1] = arr[aux];
                aux--;
            }
            arr[aux + 1] = temp;
            System.out.println(i + 1 + "회전 : " + Arrays.toString(arr));
        }
    }
}

/* 출력
리스트 : [9, 6, 7, 3, 5]
2회전 : [6, 9, 7, 3, 5]
3회전 : [6, 7, 9, 3, 5]
4회전 : [3, 6, 7, 9, 5]
5회전 : [3, 5, 6, 7, 9]
*/
```

[참고: 위키백과](https://ko.wikipedia.org/wiki/%EC%82%BD%EC%9E%85_%EC%A0%95%EB%A0%AC)