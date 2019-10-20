---
title: "[Java] 조합(Combination)"
layout: post
date: 2019-10-18 19:10
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Algorithm
- Java
category: blog
author: kyudongPark
description: 조합(Combination)에 대해서 정리한 글
---

이번 프스트에는 **조합(Combination)** 에 대해 알아보도록 하겠습니다. 

## 조합(Combination) 

알고리즘 문제를 풀다 보면 조합과 순열을 이용해서 문제를 풀어야 하는 경우를 다수 보게 된다. 먼저 조합에 대해 알아보겠습니다.  

우선, 조합의 정의는  
> **서로 다른 n개의 원소 중에서 k개를 뽑는(취하는) 것**

우리는 여기서 순서와 상관없이 뽑는 경우를 다룰 것입니다.

2가지 경우가 있다. 
* **중복이 없는 조합**
* **중복이 있는 조합 (중복 조합)**

우선 중복이 없는 조합에 대해 살펴보겠습니다.

점화식을 사용하여 조합을 구하는 공식은 (n은 원소의 갯수, r은 뽑는 갯수)
> **nCr = <em><sub>n-1</sub></em>C<em><sub>r-1</sub></em> + <em><sub>n-1</sub></em>C<em><sub>r</sub></em>**


중, 고등학생 때에는 점화식을 이용하기 보다는 따로 팩토리얼을 사용해서 풀었었지만 앞으로는 위 점화식을 이용할 것이다.  

점화식이 위와 같이 나올 수 있는 이유는 다음과 같다. 

* **특정한 원소를 포함하고 뽑을 때**
* **특정한 원소를 포함하지 않고 뽑을 때**

예를 들어,  
{ 1 2 3 4 } 4개의 원소가 있고 이 중 2개의 숫자를 뽑는다고 하면 <sub>4</sub>C<sub>2</sub>가 된다.  
나올 수 있는 경우의 수는 (1 2), (1 3), (1 4), (2 3), (2 4), (3, 4) 이다.  

이 경우는 위와 같이 특정한 원소를 포함하거나 포함하지 않고 뽑는 경우로 나눌 수 있다.  

<br>

* ( 1  2 ) , ( 1  3 ) , ( 1  4 ) 의 1을 **포함하는 경우**
* ( 2  3 ), ( 2  4 ), ( 3  4 ) 와 같이 1을 **포함하지 않는 경우**

<br>

1을 포함하는 경우에는 ( 1  ? )와 같이 1을 이미 뽑았으므로 1을 제외한 {  2  3  4  } 3개의 원소 중에서 1개의 숫자를 뽑으면 된다. 따라서 <sub>3</sub>C<sub>1</sub>로 표현할 수 있다.  
1을 포함하지 않는 경우는 ( ?  ? )와 같이 1을 제외한 {  2  3  4  } 3개의 원소 중에서 2개의 숫자를 뽑으면 된다. 따라서 
<sub>3</sub>C<sub>2</sub>로 표현할 수 있다. 

따라서 **<sub>4</sub>C<sub>2</sub> = <sub>3</sub>C<sub>1</sub> + <sub>3</sub>C<sub>2</sub>** 가 성립한다. 

즉, **트리** 의 형태로 보게 된다면  
우선, 1을 선택한 경우와 선택하지 않은 경우로 나뉜다. 먼저 1을 선택한 경우에는 다음 뽑는 원소로 2를 선택할 경우와 선택하지 않을 경우로 나뉘게 된다. 1을 선택하지 않은 경우에는 넘기고 2를 선택할 경우와 선택하지 않는 경우로 나뉜다. 이렇게 쭉쭉 트리의 형태로 뻗어나가게 됩니다. 

트리의 형태로 도식화하면 
![combinationTree](../assets/images/combTree.jpeg)

( 1 2 )  ( 1 3 )  ( 1 4 )  ( 2 3 )  ( 2 4 )  ( 3 4 ) 의 6가지 경우가 나오게 됩니다. 
    

우리는 이것을 재귀함수를 사용하여 모든 경우를 탐색할 것입니다.  
**재귀(recursion)** 에서 중요한 것은 재귀의 **종료와 탈출 조건** 이 있어야 하는 것입니다. 위 트리를 보면 원소가 4개뿐이라 쉽지만 갯수가 많아질수록 더 복잡한 트리의 형태가 나타나게 됩니다.  
그렇다면 위 트리에서 종료와 탈출 조건은 무엇일까요?

* **2개를 모두 뽑았을 경우**
* **뽑든 안뽑든 탐색을 모두 마쳤을 경우**


2개를 모두 뽑았을 경우 **r은 0** 이 되어 더 이상 뽑을 수 없습니다. 또한 탐색을 모두 마쳤을 경우에도 종료하게 됩니다. 

보통 알고리즘 문제에서는 2가지 경우를 물어봅니다. **조합의 갯수** 와 **조합의 나열**

먼저 **조합의 갯수** 는

```java
    static int combinationNum(int n, int r) {
        if (r == 0 || n == r) {
            return 1;
        } else {
            // 재귀(recursion) , nCr = n-1Cr-1 + n-1Cr
            return combinationNum(n - 1, r - 1) + combinationNum(n - 1, r);
        }
    }
```

의 코드를 통해 구할 수 있습니다.

<br>

**조합의 나열** 은 갯수를 구하는 것보다 아주 조금 복잡합니다. 

```java
    public static void main(String[] args) {
        int[] list = new int[2];
        printCombination(list, 5, 2, 0, 1);
    }

    static void printCombination(int[] arr, int n, int r, int index, int target) {
        // 2개의 원소를 다 뽑아 r이 0이 된 경우 
        if (r == 0) {
            for(int i : arr) {
                System.out.print(i + " ");
            }
            System.out.println();
            return;
        }
        
        // 2개의 원소를 다 뽑지 못하였더라도 모든 탐색을 마쳐서 종료하는 경우 
        if (n == target) return;

        arr[index] = target;
        
        // 뽑았으므로 index 1증가, 뽑아야하는 갯수 1 감소 (r-1)
        // 원소를 결정하였으므로 target 1 증가 
        printCombination(arr, n, r - 1, index + 1, target + 1);
        
        // 뽑지 않았으므로 index는 그대로, r도 그대로
        // 원소를 결정하지 않았으므로 target 1 증가
        printCombination(arr, n, r, index, target + 1);
    }
```

* **arr** 은 뽑는 갯수만큼의 공간을 가지는 배열
* **index** 는 구한 원소의 갯수
* **target** 은 탐색의 종료를 위해 어느것부터 원소를 선택할지
* **n** 은 원소의 갯수, **r** 은 뽑는 갯수

<br>

만약 {  1  2  3  4  } 에서 (  1  ?  ) 와 같이 1을 뽑았으면 arr배열의 첫 원소는 뽑은 것이기 때문에 index를 1 증가시킨다. 다음 원소로는 {  2  3  4  } 중 한 개를 뽑는 것이기 때문에 r은 1 감소시킨다. 또한, 원소를 결정하였으므로 target도 1을 증가시킨다.  
만약 {  1  2  3  4  } 에서 (  ?  ?  ) 와 같이 1을 뽑지 않았으면 arr배열의 첫 원소는 뽑은 것이 아니기 때문에 index는 그대로 두고 r 또한 그대로 둔다. 
1을 뽑지 않았으므로 target은 1 증가시켜 (  2  ?  ) 와 같이 2부터 뽑을 수 있도록 한다. 




## 중복 조합

중복조합은 
> **<sub>n</sub>H<sub>r</sub>** 입니다.

{  1  2  3  4  } 4개의 원소 중에서 중복을 허용하여 뽑게 된다면  
( 1 1 ) ( 1 2 ) ( 1 3 ) ( 1 4 ) ( 2 2 ) ( 2 3 ) ( 2 4 ) ( 3 3 ) ( 3 4 ) ( 4 4 ) 총 10가지 경우가 나온다. 

```java
    static void combinationH(int[] arr, int n, int r, int index, int target) {
        if (r == 0) {
            for(int i : arr) {
                System.out.print(i + " ");
            }
            System.out.println();
            return;
        }
        if (n == target) return;

        arr[index] = target;
        combinationH(arr, n, r - 1, index + 1, target);
        combinationH(arr, n, r, index, target + 1);
    }
```

조합과의 차이점은 원소를 뽑았어도 target을 증가시키지 않는 것입니다. 
