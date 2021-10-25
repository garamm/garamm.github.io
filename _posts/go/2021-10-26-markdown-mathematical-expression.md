---
layout: post
title: "[Markdown] LaTeX로 markdown에 수식 작성하기"
author: 임가람
date: "2021-10-26 00:39:00"
categories: [Markdown]
tags: [Markdown, 수식]
---

## LaTeX란?
[뇌를 자극하는 알고리즘]() 책을 읽고 포스팅을 하다보니 알고리즘 시간 복잡도를 계산하는 부분에서 수식이 필요하게 됐다.<br>
처음에는 수식마다 이미지를 만들어야겠다는 마음으로 있었는데, 검색해보니 `LaTex` 문법이다.<br>
LaTeX는 문서 작성 도구의 일종으로, 논문이나 출판물 등의 특수 형식 문서를 작성하는 데 쓰이는 시스템이다. 자연과학이나 인문과학중 수식, 그래프, 다이어그램을 많이 그리는 학자들에게 유용한 문서 저작도구라고 한다.(참고: [나무위키](https://namu.wiki/w/LaTeX))<br>
<br>

---

<br>

## jekyll에서 수식 사용할 수 있도록 설정하기
처음에 LaTeX 문법을 사용해서 포스팅을 작성한 후 jekyll을 실행해봤더니 수식이 적용되지 않아서 당황했다.<br>
LaTeX 문법을 사용하기 위해서는 아래와 같은 과정을 거쳐야 한다.<br>
참고: [MKKIM's Blog](https://mkkim85.github.io/blog-apply-mathjax-to-jekyll-and-github-pages/)

<br>

---
<br>

## 수식 정렬

### 왼쪽 정렬(default)
수식을 `$`와 `$` 사이에 입력한다.<br>
ex. \$2^2 = 4\$<br>
$2^2 = 4$
<br>

### 가운데 정렬
수식을 `$$`와 `$$` 사이에 입력한다.<br>
ex. \$\$2^2 = 4\$\$<br>

$$ 2^2 = 4 $$
<br>

### 특정 문자를 기준으로 정렬
보통 `=`을 기준으로 정렬하게 되는데, 이 때 가운데 정렬을 하면 아래와 같이 보인다.
ex.<br>
\$\$<br>
a+b = 5 \\\\<br>
x+y+z = 10<br>
\$\$

$$
a+b = 5 \\
x+y+z = 10
$$

그래서 `aligned`으로 수식을 감싸주고, 정렬 기준으로 삼을 문자 앞에 `&`을 입력해 더 보기 좋게 정렬할 수 있다.<br>

\$\$<br>
\begin{aligned}<br>
&a+b = 5 \\\\<br>
&x+y+z = 10<br>
\end{aligned}<br>
\$\$

$$
\begin{aligned}
&a+b = 5 \\
&x+y+z = 10
\end{aligned}
$$


---
<br>

## 줄바꿈
수식 내에서 줄바꿈이 필요한 경우 `\\`를 입력한다.<br>
jekyll markdown에서는 `\\\\\\`을 입력해야 적용된다.<br>
ex. \$2^2 = 4 \\\\ 3^2 = 9\$<br>

$2^2 = 4 \\ 3^2 = 9$
<br>
---
<br>


## 수식 작성하기

### 기본 연산
나누기: \div (Ex. X\div Y)
$$ X\div Y $$

곱하기: \times (Ex. X\times Y)
$$ X\times Y $$

### 분수
\frac (Ex. \frac{X}{Y})
$$ \frac{X}{Y} $$

### 루트
\sqrt (Ex. \sqrt{XY})
$$ \sqrt{XY} $$


# 첨자
위첨자: ^{내용} (Ex. XY^2)
$$ XY^2 $$
XY^22이면 아래와 같이 나오므로 `{ }` 괄호로 묶어줘야 함
$$ XY^22 $$
$$ XY^{22} $$


### 시간 복잡도

|이름|기호|입력방법|
|---|---|---|
|Big O|$O$|O|
|Big Omega|$\Omega$|\Omega|
|Big Theta|$\Theta$|\Theta|

### 줄임표 (Ellipsis)

|이름|기호|입력방법|
|---|---|---|
|밑 줄임표|$\ldots$|\ldots|
|가운데 줄임표|$\cdots$|\cdots|
|세로 줄임표|$\vdots$|\vdots|
|대각선 줄임표|$\ddots$|\ddots|

### 부등호

|기호|입력방법|
|---|---|
|$<$|<|
|$>$|>|
|$\le$|\le|
|$\ge$|\ge|
|$\ll$|\ll|
|$\gg$|\gg|



참고 사이트<br>
[https://velog.io/@d2h10s/LaTex-Markdown-수식-작성법](https://velog.io/@d2h10s/LaTex-Markdown-수식-작성법)
[https://texblog.org/2014/06/24/big-o-and-related-notations-in-latex/](https://texblog.org/2014/06/24/big-o-and-related-notations-in-latex/)<br>
[https://latex-tutorial.com/ellipses-in-latex/](https://latex-tutorial.com/ellipses-in-latex/)<br>
[https://latex-tutorial.com/less-than-and-greater-than-symbols/](https://latex-tutorial.com/less-than-and-greater-than-symbols/)<br>
[위키백과](https://ko.wikipedia.org/wiki/위키백과:TeX_문법)<br>
[제타위키](https://zetawiki.com/wiki/TeX_문법)<br>
