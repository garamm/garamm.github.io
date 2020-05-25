---
layout: book
title: "힙정렬"
date: "2020-05-25"
---

### 힙
 - 최댓값 및 최솟값을 찾아내는 연산을 빠르게 하기 위해 고안된 완전이진트리를 기본으로 한 자료구조
   - A가 B의 부모노드이면, A의 키값 사이에는 대소관계가 성립
   - 부모노드의 키값이 자식노드의 키값보다 항상 큰 힙을 '최대 힙', 부모노드의 키값이 자식노드의 키값보다 항상 작은 힙을 '최소 힙'이라고 부름
   - 키값의 대소관계는 오로지 부모노드와 자식노드 간에만 성립하며, 특히 형제 사이에는 대소관계가 정해지지 않음
   - 각 노드의 자식노드의 최대개수는 힙의 종류에 따라 다르지만, 대부분의 경우는 자식노드의 개수가 최대 2개인 이진 힙(binary heap)을 사용

<br><br>

### 완전이진트리
 - 데이터가 부모노드부터 시작해서 자식 노드가 왼쪽, 오른쪽 순서로 들어기는 구조의 이진 트리
 
 <br><br>
 
### 정렬 방법
 - 완전 이진 트리는 순서대로 삽입되기 때문에 정렬할 데이터를 배열에 그대로 삽입한다. (배열의 인덱스를 표기 순서로 인식)

<br><br>

### 소스코드

```java
public static void main(String[] args) {

    int[] heap = {7, 6, 5, 8, 3, 5, 9, 1, 6};
    // 힙 구성
    for (int i = 1; i < heap.length; i++) {
        int c = i;
        do {
            int root = (c - 1) / 2;
            if (heap[root] < heap[c]) {
                int temp = heap[root];
                heap[root] = heap[c];
                heap[c] = temp;
            }
            c = root;
        } while (c != 0);
    }

    // 크기를 줄여가며 반복적으로 힙 구성
    for (int i = heap.length - 1; i >= 0; i--) {
        int temp = heap[0];
        heap[0] = heap[i];
        heap[i] = temp;
        int root = 0;
        int c = 1;
        do {
            c = 2 * root + 1;
            // 자식노드 중에 더 큰 값 찾기
            if (c < i - 1 && heap[c] < heap[c + 1]) {
                c++;
            }
            // 루트보다 자식노드가 크면 교환
            if (c < i && heap[root] < heap[c]) {
                temp = heap[root];
                heap[root] = heap[c];
                heap[c] = temp;
            }
            root = c;
        } while (c < i);
    }

    // 결과 출력
    System.out.println(Arrays.toString(heap));
}
```

[참고1](https://ko.wikipedia.org/wiki/%ED%9E%99_(%EC%9E%90%EB%A3%8C_%EA%B5%AC%EC%A1%B0))
[참고2](https://blog.naver.com/ndb796/221228342808)