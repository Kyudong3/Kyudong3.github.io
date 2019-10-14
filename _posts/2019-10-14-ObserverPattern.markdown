---
title: "[디자인패턴] 옵저버 패턴(Observer Pattern)"
layout: post
date: 2019-10-14 16:10
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Java
- pattern
category: blog
author: kyudongPark
description: 옵저버 패턴이란?
---

이번에는 디자인 패턴 중에 하나인 **옵저버 패턴(Observer Pattern)** 에 대해 알아보도록 하겠습니다. 

## 옵저버 패턴?
-------------
한 객체의 상태 변화에 따라 다른 객체의 상태도 변화하도록 일대다 객체 의존 관계를 구성하는 패턴입니다. Subject에 observer들이 일대다로 연결되며 의존합니다. 

신문 구독의 예를 들어 옵저버 패턴(Observer Pattern)을 한 문장으로 표현하자면
* Observer Pattern = Publisher(출판사) + Subscriber(구독자) 
입니다.

이해하기 쉽게 그림으로 표현하면 
![observerPatternImage](../assets/images/observerPattern.jpeg)

이렇습니다.



## 언제 사용될까요?
-------------
* 데이터의 변경이 발생했을 경우 상대 클래스나 객체에 의존하지 않으면서 데이터 변경을 통보할 때(일방적인 통지방식을 사용합니다)
* 한 객체의 변경으로 변경되어져야 하는 객체 수가 많을 때 (유지 보수 쉽다)
subject는 특정 데이터를 감시하고 있다가 변화를 감지한다. 

## 어떻게 사용할까요?
-------------
보통 API를 통해 사용합니다. 하지만 여기서는 원리를 알기 위해서 직접 구현해서 하도록 하겠습니다. 

## 단점이 있나요?
-------------
* Observable은 클래스입니다.
* Observable 클래스의 핵심 메소드를 외부 호출 불가능

옵저버 기능을 추가하고자 하는 클래스가 이미 다른 클래스를 확장하고 있다면 그 클래스는 Observable의 기능을 추가할 수 없습니다. 
또한 setChanged()와 같은 핵심 메소드는 protected 접근 제한자로 선언되어 있어 외부에서 호출이 불가능합니다. 


```java
Person("KyuDong", 27, "Korea").let {
  println(it)
  it.moveTo("London")
  it.incrementAge()
  println(it)
}
```

## 결론
-------------



