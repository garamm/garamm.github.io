---
layout: post
category: "algorithm-solve"
title: "[프로그래머스] 완전탐색/모의고사"
date: "2020-10-22"
---

### 문제 설명
수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.<br>
<br>
1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...<br>
2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...<br>
3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...<br>
<br>
1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.

<br>

### 제한사항
- 시험은 최대 10,000 문제로 구성되어있습니다.
- 문제의 정답은 1, 2, 3, 4, 5중 하나입니다.
- 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.

<br>

### 입출력 예

|answers|return|
|---|---|
|[1,2,3,4,5]|[1]|
|[1,3,2,4,2]|[1,2,3]|

입출력 예 설명<br>
- 입출력 예 #1
  - 수포자 1은 모든 문제를 맞혔습니다.
  - 수포자 2는 모든 문제를 틀렸습니다.
  - 수포자 3은 모든 문제를 틀렸습니다.
  - 따라서 가장 문제를 많이 맞힌 사람은 수포자 1입니다.
- 입출력 예 #2
  - 모든 사람이 2문제씩을 맞췄습니다.

<br>

### 문제풀이

```go
func solution(answers []int) []int {
	student1 := [5]int{1, 2, 3, 4, 5}
	student2 := [8]int{2, 1, 2, 3, 2, 4, 2, 5}
	student3 := [10]int{3, 3, 1, 1, 2, 2, 4, 4, 5, 5}
	scores := [3]int{0, 0, 0}

	// 답이 맞는지 비교
	for i := 0; i < len(answers); i++ {
		var answer = answers[i];
		if(student1[i%len(student1)] == answer) {
			scores[0]++
		}
		if(student2[i%len(student2)] == answer) {
			scores[1]++
		}
		if(student3[i%len(student3)] == answer) {
			scores[2]++
		}
	}

	// 최대값 구하기
	max := 0
	for i := 0; i < len(scores); i++ {
		if max < scores[i] {
			max = scores[i]
		}		
	}

	// 최대값이랑 같은 학생 구하기
	result := []int{}
	for i := 0; i < len(scores); i++ {
		if max == scores[i] {
			result = append(result, i+1);
		}		
	}

    return result
}
```