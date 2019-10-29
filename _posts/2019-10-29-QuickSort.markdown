---
title: "[자료구조] 퀵소트(QuickSort)"
layout: post
date: 2019-10-29 15:48
image: /assets/images/markdown.jpg
headerImage: false
tag:
- data structure
- algorithm
category: blog
author: kyudongPark
description: about QuickSort
---

이번 포스트에서는 정렬 알고리즘 중 하나인 퀵정렬(QuickSort)에 대해 알아보겠습니다.

우선 QuickSort는 무엇일까요??

QuickSort는 다른 원소와 비교하면서 정렬을 수행하는 비교 정렬입니다. 분할 정복(Divide and conquer) 알고리즘의 하나로, 평균적으로 매우 빠른 수행 속도를 자랑하는 정렬 방법입니다.  
그렇다면 분할 정복은 무엇일까요? 분할 정복(divide and conquer)은  

* 문제를 2개 이상의 작은 문제로 나누고 ( divide )
* 각각의 작은 문제를 해결한 다음 ( conquer ) 
* 합병하여 원래의 문제를 해결하는 전략 ( combine )

분할 정복 방법은 보통 재귀를 사용하여 해결합니다. 

퀵 소트는 일정한 기준(pivot)을 정하고 기준보다 더 큰 값과, 더 작은 값들로 나누어 정렬하는 방법입니다.  
글로는 이해가 잘 안될수도 있으니 아래의 그림을 통해서 좀 더 알아보겠습니다!






