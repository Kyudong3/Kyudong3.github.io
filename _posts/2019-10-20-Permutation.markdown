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

<br>

우리는 다음과 같은 방법을 사용하여 구현할 수 있다.

**1. Swap을 이용**
Swap 함수를 이용하는 것은 순서 없이 n 개 중에서 r 개를 뽑는 경우이다. 
학교에서 처음 씨언어 배울 때 수도 없이 봤던 Swap... 아시나요? 바로 배열의 순서를 바꾸는 것입니다.  
배열의 처음 값부터 순서대로 하나씩 swap 한다. 

우리는  
> static void permutation(int[] arr, int depth, int n, int r) 
를 사용합니다. 

* **arr** 은 원소가 있는 배열 
* **depth** 는 어떤 깊이에서 swap 을 하는지를 나타내는 기준
* **r** 은 몇개를 뽑아서 순열을 만들 것인지

```java
    static void permutation(int[] arr, int depth, int n, int r) {
        if(depth == r) {
            for(int i=0; i<r; i++) {
                System.out.print(arr[i] + " ");
            }
            System.out.println();
            return;
        }

        for(int i=depth; i<n; i++) {
            swap(arr, depth, i);
            permutation(arr, depth + 1, n, r);
            swap(arr, depth, i);
        }
    }

    static void swap(int[] arr, int depth, int i) {
        int temp = arr[depth];
        arr[depth] = arr[i];
        arr[i] = temp;
    }
```

예를 들어,  
1 2 3 4 를 원소로 가지는 배열 **{ 1 2 3 4 }** 이 있고, 4개를 뽑아 순열을 만들고 싶다.  
기억할 것은 **depth** 가 기준이 되어 swap을 하는 것이다. 

depth에 따라  
0일 때 -> 1 X X X
1일 때 -> 1 2 X X , 1 3 X X , 1 4 X X
2일 때 -> 1 2 3 X
3일 때 -> 1 2 3 4
4일 때 -> 출력 
가 됩니다. 

<br>

depth가  
0일 때 -> 배열의 0번째 자리인 1과 **i** 번째 원소인 1을 swap 하면 { 1 X X X } 이 되고 depth를 1 증가시킨다.  
1일 때 -> { 1 X Y Z } 에서 1번째 자리인 X 에서 swap 이 일어난다. { 1 2 3 4 } 배열의 1번째 원소인 2와 **i** 번째 원소인 2를 swap 한다.  
2일 때 -> { 1 2 Y Z } 에서 2번째 자리인 Y 에서 swap 이 일어난다. { 1 2 3 4 } 배열의 2번째 원소인 3과 **i** 번째 원소인 3을 swap 한다.  

...
...

이렇게 쭉 가면서 순열을 만들게 된다.  

그런데 왜 swap 이 2번 나오는 것일까? 의문을 갖을 수 있다.  
뒤에 나오는 swap 은 배열의 순서를 기억하고 초기화하기 위해서 필요한 것이다.  
트리의 형태로 보면 dfs 로 진행되기 때문에 depth가 0 1 2 3 2 3 1 2 3 ... 처럼 계속 변하기 때문에 permutation 메소드 내부에서 가지고 있는
배열의 순서가 바뀌어져 있기 때문에 순서를 기억해야 한다. 

하지만 Swap 을 이용한 방법은 순서를 보장하지 않는다. 
