---
title: "[Java] 순열(Permutation)"
layout: post
date: 2019-10-20 22:30
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Algorithm
- Java
category: blog
author: kyudongPark
description: 순열(Permutation)에 대해서 정리한 글
---

이번 프스트에는 **순열(Permutation)** 에 대해 알아보도록 하겠습니다. 

## 순열 (Permutation) 

이번 시간에는 순열에 대해 알아보겠습니다. 

우선, 순열의 정의는  
> **서로 다른 n개의 원소에서 r개를 중복없이 골라 순서에 상관있게 나열하는 것을 n개에서 r개를 택하는 순열이라고 한다**

순열은 아래와 같이 표현할 수 있다. (n은 원소의 갯수, r은 뽑는 갯수)
> **nPr = n * (n - 1) * (n - 2) ... (n - r + 1)**<em><sub>n-1</sub></em>C<em><sub>r-1</sub></em> + <em><sub>n-1</sub></em>C<em><sub>r</sub></em>**
> **nPr = <sub>n-1</sub>P<sub>r-1</sub>**

순열에는 2가지 경우가 있다. 

* **중복이 없는 순열**
* **중복이 있는 순열 (중복순열)**


예를 들어,  
1 2 3 이 적힌 3개의 공이 있다.  
이 중 **중복을 허용하지 않고** 2개의 공을 뽑는다고 하면 <sub>3</sub>P<sub>2</sub>가 된다.  
나올 수 있는 경우의 수는 
( 1  2 )  
( 1  3 )  
( 2  1 )  
( 2  3 )  
( 3  1 )  
( 3  2 )  
의 6가지 ( 3 * 2 ) 이다.  

**중복을 허용** 하고 2개의 공을 뽑는 경우에 나올 수 있는 경우의 수는 
( 1  1 )
( 1  2 )
( 1  3 )
( 2  1 )
( 2  2 )
( 2  3 )
( 3  1 )
( 3  2 )
( 3  3 )  
의 9가지 ( 3 * 3 ) 이다. 


다음과 같이 코드로 구현할 수 있다.

<br>

**1. Swap을 이용**
학교에서 처음 씨언어 배울 때 수도 없이 봤던 Swap... 아시나요? 바로 배열의 순서를 바꾸는 것입니다. 

