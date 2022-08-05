---
layout: post
title: "[Markdown] LaTeX, Mathjax로 markdown에 수식 작성하기"
author: 임가람
date: "2021-10-26 00:52:00"
categories: [Markdown]
tags: [Markdown, 수식]
---


## LaTeX란?
[뇌를 자극하는 알고리즘](https://garamm.github.io/categories/뇌를-자극하는-알고리즘){:target="blank"} 책을 읽고 포스팅을 하다보니 알고리즘 시간 복잡도를 계산하는 부분에서 수식이 필요하게 됐다.<br>
처음에는 수식마다 이미지를 만들어야겠다는 마음으로 있었는데, 검색해보니 `LaTex` 문법이다.<br>
LaTeX는 문서 작성 도구의 일종으로, 논문이나 출판물 등의 특수 형식 문서를 작성하는 데 쓰이는 시스템이다. 자연과학이나 인문과학중 수식, 그래프, 다이어그램을 많이 그리는 학자들에게 유용한 문서 저작도구라고 한다.(참고: [나무위키](https://namu.wiki/w/LaTeX){:target="blank"})<br>
<br>

---

<br>

## jekyll에서 수식 사용할 수 있도록 설정하기
처음에 LaTeX 문법을 사용해서 포스팅을 작성한 후 jekyll을 실행해봤더니 수식이 적용되지 않아서 당황했다.<br>
LaTeX 문법을 사용하기 위해서는 아래와 같은 과정을 거쳐야 한다.<br>
참고: [MKKIM's Blog](https://mkkim85.github.io/blog-apply-mathjax-to-jekyll-and-github-pages/){:target="blank"}

<br>

---

<br>
## 수식 정렬

### 왼쪽 정렬(default)
수식을 `$`와 `$` 사이에 입력한다.<br>
```
$2^2 = 4$
```
$2^2 = 4$
<br>

### 가운데 정렬
수식을 `$$`와 `$$` 사이에 입력한다.<br>
```
$$
2^2 = 4
$$
```

$$
2^2 = 4
$$

<br>

<br>

### 특정 문자를 기준으로 정렬
일반적으로 수식에서는 `=`을 기준으로 정렬하게 되는데, 이 때 가운데 정렬을 하면 아래와 같이 보인다.

```
$$
a+b = 5 \\
x+y+z = 10
$$
```

$$
a+b = 5 \\
x+y+z = 10
$$

그래서 `aligned`으로 수식을 감싸주고, 정렬 기준으로 삼을 문자 앞에 `&`을 입력해 더 보기 좋게 정렬할 수 있다.<br>

```
$$
\begin{aligned}
&a+b = 5 \\\\\\
&x+y+z = 10
\end{aligned}
$$
```

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
그런데 현재 블로그에서는 적용이 안되서 `\\\\\\`을 입력했다.<br>
```
$
2^2 = 4 \\\\\\
3^2 = 9
$
```

$
2^2 = 4 \\\\\\
3^2 = 9
$

<br>
---
<br>


## 수식 작성하기

### 기본 연산
나누기
```
$$X \div Y$$
```

$$X \div Y$$

곱하기
```
$$ X \times Y $$
```

$$ X \times Y $$

### 분수
일반 표기
```
$$ \frac{X}{Y} $$
```

$$ \frac{X}{Y} $$

크게 표기
```
$$ \dfrac{X}{Y} $$
```

$$ \dfrac{X}{Y} $$

괄호 표기
```
$$ ( \frac{1}{2} ) $$
```

$$ ( \frac{1}{2} ) $$

위와 같이 표기하는 것 보다 아래 처럼 표기하는 것이 좋음
```
$$ \left( \frac{1}{2} \right) $$
```

$$ \left( \frac{1}{2} \right) $$

### 루트
```
$$ \sqrt{XY} $$
```

$$ \sqrt{XY} $$


# 첨자
위첨자
```
$$ XY^2 $$
```

$$ XY^2 $$

XY^22이면 $ XY^22 $ 이렇게 출력되므로 `{ }` 괄호로 묶어줘야 함
```
$$ XY^{22} $$
```

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

### 시그마
```
$$
\sum_{k=1}^2 {k+1}
$$
```

$$
\sum_{k=1}^2 {k+1}
$$

### 행렬
```
$$ \begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix}	 $$
```

$$ \begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix}	 $$

```
$$
\begin{bmatrix}
0 & \cdots & 0 \\
\vdots & \ddots & \vdots \\
0 & \cdots & 0
\end{bmatrix}
$$
```

$$
\begin{bmatrix}
0 & \cdots & 0 \\
\vdots & \ddots & \vdots \\
0 & \cdots & 0
\end{bmatrix}
$$

### 방정식
```
$$
f(n)=
\begin{cases}
n/2, & \mbox n=0 \\
3n+1, & \mbox n>0
\end{cases}
$$
```

$$
f(n)=
\begin{cases}
n/2, & \mbox n=0 \\
3n+1, & \mbox n>0
\end{cases}
$$

### 문자 쓰기
```
$$
\text{LCS_LENGTH} \\\\\\
\texttt{LCS_LENGTH}
$$
```

$$
\text{LCS_LENGTH} \\\\\\
\texttt{LCS_LENGTH}
$$

<br>

참고 사이트<br>
[https://velog.io/@d2h10s/LaTex-Markdown-수식-작성법](https://velog.io/@d2h10s/LaTex-Markdown-수식-작성법){:target="_blank"}<br>
[https://texblog.org/2014/06/24/big-o-and-related-notations-in-latex/](https://texblog.org/2014/06/24/big-o-and-related-notations-in-latex/){:target="_blank"}<br>
[https://latex-tutorial.com/ellipses-in-latex/](https://latex-tutorial.com/ellipses-in-latex/){:target="_blank"}<br>
[https://latex-tutorial.com/less-than-and-greater-than-symbols/](https://latex-tutorial.com/less-than-and-greater-than-symbols/){:target="_blank"}<br>
[위키백과](https://ko.wikipedia.org/wiki/위키백과:TeX_문법){:target="_blank"}<br>
[제타위키](https://zetawiki.com/wiki/TeX_문법){:target="_blank"}<br>
[https://math.meta.stackexchange.com/](https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference){:target="_blank"}<br>