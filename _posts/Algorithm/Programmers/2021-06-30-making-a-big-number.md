---
layout: post
title: "[프로그래머스] 탐욕법/큰 수 만들기"
date: "2021-06-30 20:12"
updated: []
categories: ["Algorithm", "Programmers"]
---

#### **# 문제 설명**<br>
어떤 숫자에서 k개의 수를 제거했을 때 얻을 수 있는 가장 큰 숫자를 구하려 합니다.<br>
예를 들어, 숫자 1924에서 수 두 개를 제거하면 [19, 12, 14, 92, 94, 24] 를 만들 수 있습니다. 이 중 가장 큰 숫자는 94 입니다.<br>
문자열 형식으로 숫자 number와 제거할 수의 개수 k가 solution 함수의 매개변수로 주어집니다. number에서 k 개의 수를 제거했을 때 만들 수 있는 수 중 가장 큰 숫자를 문자열 형태로 return 하도록 solution 함수를 완성하세요.<br>
<br>
#### **# 제한사항**<br>
- number는 2자리 이상, 1,000,000자리 이하인 숫자입니다.
- k는 1 이상 number의 자릿수 미만인 자연수입니다.

<br>
#### **# 입출력 예**

| number | k | return |
| --- | --- | --- |
| "1924" | 2 | "94" |
| "1231234" | 3 | "3234" |
| "4177252841" | 4 | "775841 |

[출처](http://hsin.hr/coci/archive/2011_2012/contest4_tasks.pdf){:target="_blank"}

---

#### **# 문제풀이**
```python
# Python3
def solution(number, k):
  answer = ''

  numList = [number[0]] # 우선 첫번째 숫자를 리스트에 넣는다
  for i in range(1, len(number)):

    if k == 0: # 숫자를 다 제외했으면 남은 숫자를 numList에 넣고 종료
      numList.append(number[i:])
      break

    # numList에 넣은 숫자랑 지금 숫자를 비교해서
    # numList에 넣은 수가 작으면 빼고 k를 줄임
    while number[i] > numList[-1]:
      numList.pop()
      k -= 1
      if len(numList) == 0 or k == 0: # numList가 비어있거나 숫자를 다 제외했으면 종료
        break
    # 현재 숫자를 numList에 넣는다.
    numList.append(number[i])
  
  answer = ''.join(numList) # 리스트를 string으로 형변환
  # for문을 끝냈는데 k가 남아있으면 남은 뒷자리 수를 자른다.
  if k != 0:
    answer = answer[:-k]
  print(answer)
  
  return answer

solution("1924", 2) # 94
solution("1231234", 3) # 3234
solution("4177252841", 4) # 775841
solution('77777', 1) # 7777
```