---
layout: post
title: "[프로그래머스] 완전탐색/모음사전"
date: "2023-02-12 21:40"
updated: []
categories: ["Algorithm", "Programmers"]
---

#### **# 문제 설명**<br>
사전에 알파벳 모음 'A', 'E', 'I', 'O', 'U'만을 사용하여 만들 수 있는, 길이 5 이하의 모든 단어가 수록되어 있습니다. 사전에서 첫 번째 단어는 "A"이고, 그다음은 "AA"이며, 마지막 단어는 "UUUUU"입니다.<br>
단어 하나 word가 매개변수로 주어질 때, 이 단어가 사전에서 몇 번째 단어인지 return 하도록 solution 함수를 완성해주세요.<br>
<br>
#### **# 제한사항**<br>
- word의 길이는 1 이상 5 이하입니다.
- word는 알파벳 대문자 'A', 'E', 'I', 'O', 'U'로만 이루어져 있습니다.

<br>
#### **# 입출력 예**

| word | result |
| --- | --- |
| "AAAAE" | 6 |
| "AAAE" | 10 |
| "I" | 1563 |
| "EIO" | 1189 |

<br>
#### **# 입출력 예 설명**<br>
**입출력 예 #1**<br>
사전에서 첫 번째 단어는 "A"이고, 그다음은 "AA", "AAA", "AAAA", "AAAAA", "AAAAE", ... 와 같습니다. "AAAAE"는 사전에서 6번째 단어입니다<br>
<br>
**입출력 예 #2**<br>
"AAAE"는 "A", "AA", "AAA", "AAAA", "AAAAA", "AAAAE", "AAAAI", "AAAAO", "AAAAU"의 다음인 10번째 단어입니다.

---

#### **# 문제풀이**
```python
# Python3
from itertools import product

def solution(word):
    answer = 0
    alphabet = ["A", "E", "I", "O", "U"]
    temp, temp2 = [], []
    for i in range(5):
        temp = temp + list(product(alphabet, repeat=i + 1))

    for item in temp:
        temp2.append("".join(item))

    temp2 = sorted(temp2)
    answer = temp2.index(word) + 1
    print(answer)
    return answer

solution("AAAAE")  # 6
solution("AAAE")  # 10
solution("I")  # 1563
solution("EIO")  # 1189
```