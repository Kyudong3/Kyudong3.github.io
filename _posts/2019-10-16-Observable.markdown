---
title: "[RxJava] Observable : Hot, Cold?"
layout: post
date: 2019-10-16 17:10
image: /assets/images/markdown.jpg
headerImage: false
tag:
- RxJava
category: blog
author: kyudongPark
description: RxJava의 Cold, Hot Observable에 대해 
---

이번에는 RxJava를 이해하기 위해 **Hot , Cold Observable** 에 대해 알아보도록 하겠습니다. 

<br>
<br>

## 개념 
RxJava를 공부하다 보면 Hot, Cold 라는 말을 자주 볼 수 있습니다. 이게 무슨 말일까요.... 처음 이 말을 보았을 때 저는 어리둥절 하였답니다.

먼저, 이 둘의 특징을 알아보겠습니다. 특징을 알고 나면 생각보다 이해하기 쉽습니다!

<br>


- Cold Observable
  - 일반적인 옵저버 형태
  - subscribe 하지 않으면 데이터를 방출하지 않음 
  - 처음부터 발행하는 것을 기본으로 한다 
  - 웹요청, 데이터베이스 쿼리와 파일 읽기 등이 있다 
  
<br>
<br>
  
  
- Hot Observable
  - Subscriber의 존재 여부와 상관없이 생성과 동시에 데이터 방출 
  - 여러 Subscriber에 선택적으로 고려 가능
  - Subscribe 시점으로부터 발행하는 값을 받는 걸 기본으로 한다 ( 중간부터 )
  - 멀티캐스팅 포함
  - 마우스, 키보드, 시스템 이벤트 등이 주로 사용된다 

<br>
<br>


두 Observable의 특징을 보면 감이 오시나요? 
좀 더 설명을 하자면, 아래와 같은 코드만 있을 때 
<br>




```java
Observable.just("Hello, world!"); 
```

<br>


just() 함수를 호출해도 Observer가 subscribe 하지 않으면 Cold Observable는 데이터를 방출하지 않습니다. 
반대로, Hot Observable은 Subscriber의 존재 여부와 상관없이 데이터를 방출합니다. 따라서 Hot Observable은 여러 구독자에
선택적으로 고려할 수 있습니다. 



또한 Cold Observable은 구독자가 구독하면 준비된 데이터를 처음부터 방출하게 되고, Hot Observable은 구독자가 구독한 시점부터 데이터를
방출받습니다. 






## 데이터 발행자/수신자 
추가적으로 **데이터 발행자, 수신자** 에 대해 알아보자
<br>


| 발행자     	| 수신자     	|
|----------------	|----------------	|
| Observable     	| Subscriber     	|
| Single         	| Observer       	|
| Maybe          	| Consumer       	|
| Subject        	|                	|
| Completable    	|                	|

<br>
<br>

* Subscriber(구독자) : Observable을 연결할 시 subscribe 함수를 이용하는데 이 과정에서 구독이 된다고 하여 구독자라고 부른다.



* Observer(옵저버) : Rx가 옵저버 패턴을 사용해서 항상 관찰하고 작업을 수행하며, 발신자가 Observable이 되고 데이터를 수신하는 쪽이 
옵저버가 된다. 



* Consumer(소비자) : RxJava2 부터 소비자 패턴을 이용해서 소비를 시킨다. Consumer는 작업 큐에 등록된 작업이나 데이터를 전달 받아 소비한다. 





