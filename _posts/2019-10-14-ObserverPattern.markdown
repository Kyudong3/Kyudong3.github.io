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

우리는 흔히 데이터를 찾고 싶을 때 리스트(List)를 돌면서 Iterator를 사용합니다. Iterator는 next()를 사용해 **풀(pull)** 방식으로 데이터를 가져오게 됩니다. 하지만 **Observable**은 **푸시(push)** 방식으로 데이터를 사용하는 쪽으로 데이터를 밀어 넣어주는 방식입니다. Iterator를 사용했을 때와는 반대되는 방식을 사용합니다. 

즉, **Observer** 들이 **Observable** 에 등록되어 있으면, 특정 이벤트가 발생했을 때 Observer들에게 데이터를 밀어 넣어주게 되는 패턴입니다. 

쉽게 말해서 옵저버 패턴이란, **한 객체의 상태 변화에 따라 다른 객체의 상태도 변화하도록 일대다 객체 의존 관계를 구성하는 패턴입니다.** Subject에 observer들이 일대다로 연결되며 의존합니다. 

신문 구독의 예를 들어 옵저버 패턴(Observer Pattern)을 한 문장으로 표현하자면
* Observer Pattern = Publisher(출판사) + Subscriber(구독자) 
입니다.

이해하기 쉽게 그림으로 표현하면 
![observerPatternImage](../assets/images/observerPattern.jpeg)

이렇습니다.

Subject와 Observer 객체는 **느슨하게 결합(Loose Coupling)** 되어 있습니다. 느슨한 결합(Loose Coupling)이란 서로 상호작용을 하지만 서로에 대해 잘 모르는 것입니다. 우리가 잘 아는 인터페이스나 추상클래스로 연결된 관계가 바로 느슨한 결합입니다.
Subject가 Observer에 대해 아는 것은 Observer가 특정 인터페이스를 구현한다는 것 뿐입니다.  

보통 

## When? 

* 데이터 변경시 상대 클래스나 객체에 의존하지 않으면서 데이터 변경을 통보할 때
* 한 객체의 변경으로 변경되어져야 하는 객체 수가 많을 때 (유지 보수가 쉽다)

위에서 말한 것과 같이 신문 구독의 예를 들면, 여러 **Subscriber(구독자)** 들이 한 **Publisher(출판사)** 를 구독하게 됩니다.  
만약 출판사에서 어떤 내용이 추가, 수정 등 데이터가 변경되었을 때 구독하고 있는 모든 Subscriber에게 이를 알려야 합니다. 이처럼 한 객체의 변경으로 여러 객체가 변경되어져야 할 때 사용합니다.

## How?

보통 API를 통해 사용합니다. Java에서는 Observable 클래스와 Observer 인터페이스를 제공합니다.  
원리를 알기 위해 여기서는 직접 구현해서 하도록 하겠습니다. 


## 예제

스터디를 개설할 수 있는지에 대한 예제를 통해 옵저버 패턴을 더 이해해 봅시다!

우선,  
**Observer 인터페이스** 를 정의합니다.

```java
public interface ObserverInterface {
    public void update(ObservableInterface observable);
}
```

Observable 인터페이스를 정의합니다.

```java
public interface ObservableInterface {
    public void registerObserver(ObserverInterface obj);    // 등록
    public void removeObserver(ObserverInterface obj);      // 삭제   
    public void notifyObservers();                          // 통보 
}
```

Observable 인터페이스를 구현하는 **StudyClass** (Subject, Publisher)  
StudyClass는 옵저버들을 알고 있는 Subject입니다. 옵저버를 등록(register)하거나 제거(remove)할 때 필요한 인터페이스를 제공합니다.

```java
public class StudyClass implements ObservableInterface {

    // 등록된 Observer들의 리스트 
    private ArrayList<ObserverInterface> observers;
    
    // 스터디모임 이름, 주제, 인원 
    private String studyName;
    private String studySubject;
    private int personNum;
    
    public StudyClass() {
        observers = new ArrayList<>();
    }

    // 데이터 변경 감지하고 dataChagend() 메소드 호출 
    public void setData(String studyName, String studySubject, int personNum) {
        this.studyName = studyName;
        this.studySubject = studySubject;
        this.personNum = personNum;
        dataChanged();
    }

    // Observer들에게 데이터가 변화되었다고 알림
    private void dataChanged() {
        notifyObservers();
    }

    @Override
    public void registerObserver(ObserverInterface obj) {
        observers.add(obj);
    }

    @Override
    public void removeObserver(ObserverInterface obj) {
        int index = observers.indexOf(obj);
        if(index >= 0) {
            observers.remove(index);
        }
    }

    @Override
    public void notifyObservers() {
        for (ObserverInterface observer : observers) {
            observer.update(this);
        }
    }
    
    // getter , setter 정의        
}
```

다음으로는 Observer 인터페이스를 이용하는 2개의 클래스  

**StudyInformation** 클래스  (Observer)  
StudyInformation은 
Observer 인터페이스를 이용하고 개설 인원이 10명 이하일 때 스터디 정보를 출력해주는 옵저버입니다.

```java
public class StudyInformation implements ObserverInterface {

    ObservableInterface observable;
    private String studyName;
    private String studySubject;
    private int personNum;

    public StudyInformation(ObservableInterface obj) {
        this.observable = obj;
        observable.registerObserver(this);
    }

    // Subject에서 생긴 변화에 따라 update 한다 
    @Override
    public void update(ObservableInterface observable) {
        if (observable instanceof StudyClass) {
            StudyClass obj = (StudyClass)observable;

            this.studyName = obj.getStudyName();
            this.studySubject = obj.getStudySubject();
            this.personNum = obj.getPersonNum();

            showInformation();
        }
    }

    private void showInformation() {
        if (personNum <= 10) {
            System.out.println("studyName : " + studyName + ",  studySubject : " + studySubject + ",  personNum : " + personNum);
        }
    }
}

```

**StudyOpen** 클래스  (Observer)
Observer 인터페이스를 이용하고 인원에 따라 스터디를 개설할 수 있는지 없는지 알려주는 Observer입니다. 

```java
public class StudyOpen implements ObserverInterface {

    ObservableInterface observable;
    private int personNum;

    public StudyOpen(ObservableInterface obj) {
        this.observable = obj;
        observable.registerObserver(this);
    }

    @Override
    public void update(ObservableInterface observable) {
        if (observable instanceof StudyClass) {
            StudyClass obj = (StudyClass)observable;

            personNum = obj.getPersonNum();

            showIfStudyCanOpened();
        }
    }

    private void showIfStudyCanOpened() {
        if(personNum > 10) {
            System.out.println("10명이 초과하여 스터디를 개설할 수 없습니다");
        } else {
            System.out.println(personNum + "인 스터디를 개설하였습니다.");
        }

        System.out.println();
    }
}
```

마지막으로, **ObserverPattern** (메인 클래스) 구현입니다.  
Publisher인 StudyClass, Observer인 StudyInformation과 StudyOpen 클래스를 선언합니다.

```java
public class ObserverPattern {

    static StudyClass studyClass;
    static StudyInformation studyInformation;
    static StudyOpen canOpened;

    // 스터디 Observalbe 객체 생성
    static void studySubject() {
        studyClass = new StudyClass();
    }

    // 스터디 정보, 스터디 개설 가능 객체 생성 및 Observer 등록
    static void aboutStudy() {
        studyInformation = new StudyInformation(studyClass);
        canOpened = new StudyOpen(studyClass);
    }

    public static void main(String[] args) {
        studySubject();
        aboutStudy();

        // 생성하고 싶은 스터디
        studyClass.setData("디자인 패턴", "옵저버 패턴", 5);
        studyClass.setData("디자인 패턴", "팩토리 패턴", 6);
        studyClass.setData("디자인 패턴", "옵저버 패턴", 12);
    }
}

```

이를 실행하면 
```java
studyName : 디자인 패턴,  studySubject : 옵저버 패턴,  personNum : 5
5인 스터디를 개설하였습니다.

studyName : 디자인 패턴,  studySubject : 팩토리 패턴,  personNum : 6
6인 스터디를 개설하였습니다.

10명이 초과하여 스터디를 개설할 수 없습니다
```

이러한 결과가 나오게 됩니다.


## 정리

**옵저버 패턴(Observer Pattern)** 은

* 푸시(push) 방식 사용
* 1:N 객체 의존 관계 
* 느슨한 결합(Loose Coupling)
* Subject가 Observer에 대해 아는 것은 특정 인터페이스를 구현한다는 것뿐
* Subject와 Observer는 독립적으로 재사용 가능 
* 자바에서는 Observable 클래스와 Observer 인터페이스를 제공한다

정도로 간단하게 정리할 수 있을 것 같습니다!



