---
title: 페르마의 소정리(Fermat's little theorem)
date: 2022-04-11 19:10:30 +09:00
categories: [Algorithm, CT]
tags: [algorithm, ct, fermat, 
theorem]     # TAG names should always be lowercase
use_math: true
---

## Fermat's little theorem  

### 합동식
---
대수학에서 합동식이란 숫자의 실제 크기를 불문하고, 어떤 수로 나누었을 때 나머지가 같은 수를 의미합니다.  

**<center>$a ≡ b (mod \; p)$이면  
$a$를 $p$로 나눈 나머지와, $b$를 $p$로 나눈 나머지는 같다.</center>**
<br>

### 페르마의 소정리
---
$a$가 정수이고 $p$가 소수이고 $p$가 $a$의 배수가 아닐 때 (즉, $a$가 임의의 소수 $p$의 약수가 아니므로, $a≠p$)

![fermat](/assets/img/2022-04-11/1_fermat_little_theorem.jpeg)
**<center>소수 $p$와 정수 $a$에 대해서 $a^p ≡ a (mod \; p)$<br> 
만약 $a$와 $p$가 서로소이면 $a^{p-1} ≡ 1 (mod \; p)$를 만족한다.</center>**
<br>

### 이항계수 $nCr$ 빠르게 구하기
---
주어지는 $n$과 $r$의 범위가 $O(nr)$의 시간과 공간 복잡도로 수행 가능하다면

$\binom{n}{r}$ = $\binom{n-1}{r-1}$ + $\binom{n-1}{r}$
공식을 이용합니다.
<br><br>
그러나 $n$과 $r$이 10만 ~ 100만 정도의 범위에 있을 경우에는 위의 방식을 이용하지 못합니다.  
이런 경우에는 항상 어떤 소수 $p$에 대한 나머지를 구하는 방식이므로  
$\binom{n}{r}$ ≡ $n!$ / $(r!(n-r)!)$ $mod \; p$ 공식을 이용합니다.
<br><br>
여기서 페르마의 소정리를 이용하여 $r!(n-r)!$의 **역원**을 곱해줘야 합니다.  
$a^{p-1} ≡ 1 (mod \; p)$을 만족하므로, $(mod \; p)$에 대해서 $a$의 역원은 $a^{p-2}$ 입니다.
<br><br>
따라서 $\binom{n}{r}$ ≡ $n!(r!(n-r)!)^{p-2}$ $(mod \; p)$을 만족합니다.
<br><br>

### 참고링크
---
[페르마의 소정리와 활용](https://rebro.kr/105)  
[마크다운 수식 작성법 1](https://csrgxtu.github.io/2015/03/20/Writing-Mathematic-Fomulars-in-Markdown/)  
[마크다운 수식 작성법 2](https://velog.io/@d2h10s/LaTex-Markdown-%EC%88%98%EC%8B%9D-%EC%9E%91%EC%84%B1%EB%B2%95)   
[마크다운 수식 작성법 3](https://huni0318.github.io/blog/blog-etc/2020-12-21-markdown-tutorial2/)  
[Jekyll 블로그에 MathJax로 수식 표현하기](https://mkkim85.github.io/blog-apply-mathjax-to-jekyll-and-github-pages/)