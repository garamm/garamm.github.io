---
layout: post
title: "[프로그래머스] 스택,큐/올바른 괄호"
date: "2022-10-21 22:23"
updated: []
categories: ["Algorithm", "Programmers"]
---

#### **# 문제 설명**<br>
괄호가 바르게 짝지어졌다는 것은 '(' 문자로 열렸으면 반드시 짝지어서 ')' 문자로 닫혀야 한다는 뜻입니다. 예를 들어<br>
- "()()" 또는 "(())()" 는 올바른 괄호입니다.
- ")()(" 또는 "(()(" 는 올바르지 않은 괄호입니다.

'(' 또는 ')' 로만 이루어진 문자열 s가 주어졌을 때, 문자열 s가 올바른 괄호이면 true를 return 하고, 올바르지 않은 괄호이면 false를 return 하는 solution 함수를 완성해 주세요.<br>
<br>
#### **# 제한사항**<br>
- 문자열 s의 길이 : 100,000 이하의 자연수
- 문자열 s는 '(' 또는 ')' 로만 이루어져 있습니다.

<br>
#### **# 입출력 예**

| s | answer |
| --- | --- |
| "()()" | true |
| "(())()" | true |
| ")()(" | false |
| "(()(" | false |

---

#### **# 문제풀이**
```python
# Python3
def solution(s):
    answer = True
    stack = []
    for item in list(s):
        if len(stack) == 0:  # 스택이 비어있을 때 처리
            stack.append(item)
            continue

        if item == "(":
            stack.append(item)
        else:  # ")"인 경우
            if stack[-1] == "(":  # 괄호가 올바르면 pop
                stack.pop()
            else:  # 괄호가 올바르지 않으면 push
                stack.append(item)

    if len(stack) != 0:
        answer = False

    return answer

solution("()()")  # true
solution("(())()")  # true
solution(")()(")  # false
solution("(()(")  # false
```