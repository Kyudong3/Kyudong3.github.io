---
title: "[자료구조] 퀵소트(QuickSort)"
layout: post
date: 2019-10-29 15:48
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Data structure
- Algorithm
category: blog
author: kyudongPark
description: about QuickSort
---

이번 포스트에서는 정렬 알고리즘 중 하나인 **퀵정렬(QuickSort)** 에 대해 알아보겠습니다.

우선 **QuickSort** 는 무엇일까요??

**QuickSort** 는 다른 원소와 비교하면서 정렬을 수행하는 비교 정렬입니다. **분할 정복(Divide and conquer)** 알고리즘의 하나로, 평균적으로 매우 빠른 수행 속도를 자랑하는 정렬 방법입니다.  

그렇다면 분할 정복은 무엇일까요?

**분할 정복(divide and conquer)** 은  

> 문제를 2개 이상의 작은 문제로 나누고 **( divide )**
> 각각의 작은 문제를 해결한 다음 **( conquer )**
> 합병하여 원래의 문제를 해결하는 전략 **( combine )**

분할 정복 방법은 보통 재귀를 사용하여 해결합니다. 

퀵 소트는 일정한 **기준(pivot)** 을 정하고 **시작점** , **끝점** 과 비교하며 원소를 계속 교환하며 기준보다 더 큰 값과, 더 작은 값들로 나누어 정렬하는 방법입니다.  

아래의 코드를 먼저 보도록 하겠습니다. 

> pivot - 기준값
> left - 시작 index
> right - 끝 index
> { 1 , 3 , 10 , -1 , 7 , 9 , -20 , 123 , 6 , 4 } 배열

```kotlin
fun main() {
    var arrList = arrayOf(1, 3, 10, -1, 7, 9, -20, 123, 6, 4)

    quickSort(arrList, 0, arrList.size - 1)

    for(i in arrList) {
        print("$i ")
    }
}

fun quickSort(arrList: Array<Int>, first: Int, last: Int) {
    var left = first
    var right = last
    val pivot = arrList[(first + last) / 2]

    do {
        while (arrList[left] < pivot) left++
        while (arrList[right] > pivot) right--

        if (left <= right) {
            var temp = arrList[left]
            arrList[left] = arrList[right]
            arrList[right] = temp
            left++
            right--
        }

    } while (left <= right)

    if (first < right) quickSort(arrList, first, right)

    if (last > left) quickSort(arrList, left, last)
}

// 실행값
// -20 -1 1 3 4 6 7 9 10 123 
```

여기서 기준점은 배열의 가운데 값( 7 )을 사용합니다. 
우선, 배열의 첫 인덱스인 0 을 left 로 하고 배열의 마지막 인덱스를 right 가 되게 하였습니다.  

그 이후의 과정은 아래와 같습니다. 

* left 와 right 가 교차되지 않는 동안
* 만약 배열 [ left ] 의 값이 기준( pivot )값 7 보다 작다면 교환될 이유가 없으므로 left ++
* 만약 배열 [ right ] 의 값이 기준( pivot )값 7보다 크다면 교환될 이유가 없으므로 right- -
* 배열 [ left ] 의 값이 pivot 보다 크다면 left 인덱스를 증가하지 않는다
* 배열 [ right ] 의 값이 pivot 보다 작다면 right 인덱스를 감소하지 않는다
* 두 개의 값을 교환한다 
* 위 과정을 반복해서 배열을 한 번 돌았으면 다시 새로운 기준점을 정하고 위 과정을 반복한다 

글로만 보면 이해하기 어려울 수도 있으니 아래 그림을 참고하시면 더 쉽게 이해할 수 있습니다. 

![QuickSort](../assets/images/quickSort.jpeg)



이건 pivot을 기준으로 배열을 한 번 돌았을 때의 과정을 그림으로 나타낸 것이니, 다음 그림은 각자 그려보시면서 공부하면 좋을 것 같습니다! 






