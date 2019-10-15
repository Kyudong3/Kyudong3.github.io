---
title: "[Java] 람다(Lambda)"
layout: post
date: 2019-10-15 16:10
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Java
category: blog
author: kyudongPark
description: 람다란?
---

Java8에서 추가 된 중요한 것 2가지를 뽑으라 한다면 아마 **람다** 와 **스트림** 일 것이다. 

이번에는 Java8 부터 소개된 **람다(Lambda)** 에 대해 알아보겠습니다.  

## Why Lambda?

람다에 대해 자세히 알아보기 전에 우선 Java에 람다가 왜 등장하게 되었는지 알 필요가 있습니다. 왜 람다를 사용할까요? 바로 자바에 **함수형 프로그래밍** 을 도입하기 위해서입니다. 
그렇다면 왜 함수형 프로그래밍이 필요한 것일까요? 함수형 프로그래밍은 기존 프로그래밍과는 다르게 생각하는 방법을 알려주기 때문입니다.  
자바를 예를 들어 설명하면, 자바는 객체지향 프로그래밍 언어입니다. 객체지향에서는 객체가 일급 객체가 되고 캡슐화, 상속, 다형성 등의 특징을 가지고 있습니다. 
또한 모든 데이터가 객체로 이루어져 있으며, 객체에서 상태를 저장하고 관리합니다. 객체지향에서는 함수라는 개념이 존재하지 않습니다. 반대로 함수형 프로그래밍은 상태와 데이터의 변경을 최대한 피하면서 코딩하는 것입니다. 더 간단하게 대입문이 없습니다. 
대입문이 없기에 우리는 함수에게 결과를 계산하는 것 외에 다른 효과를 기대할 수 없습니다. 이는 **부수 효과(side effect)** 가 전혀 없다는 말이 됩니다.
함수는 오직 **Input에 의해서만 Output이 달라져야 합니다.**



## 람다(Lambda)?

그렇다면 **람다(Lambda)** 는 무엇일까요?  
람다식은 자바에서 함수형 프로그래밍을 표현하기 위해 사용되며 **메소드를 하나의 식(Expression)으로 표현한 것** 입니다. 메소드 이름과 리턴타입이 사라지게 되므로 **익명함수(anonymous function)** 라고도 한다.

람다를 사용하게 되면 코드를 간결하게 만들 수 있고, 가독성이 향상되며 병렬 프로그래밍이 쉬워집니다. 

```java
// expression 사용 
(int a, int b) -> a > b ? a : b

// 매개변수 타입 생략 
(a, b) -> a > b ? a : b

// ( ) 생략
a - > a + 1
```

람다는 

* 매개변수 타입 추론 가능
* 람다식 문법을 컴파일러가 익명 클래스로 변환
* 반환값이 있는 메서드는 return 대신 식(expression)으로 대신한다
* 매개변수가 하나일 때 ( ) 생략 가능
* { } 안에 return문이 아닌데 문장이 하나일 경우 중괄호 생략 가능

의 특징을 가지고 있습니다.


## 표현식 

람다식의 기본 구조는 아래와 같습니다.   
오른쪽 중괄호 { } 블록이 있고, 이를 실행하기 위해 필요한 값인 매개변수가 있습니다. -> 는 매개변수를 이용해서 람다의 바디를 실행한다는 뜻입니다. 

```java
(int a, int b) -> { return a + b }
```

위 구조를 람다식을 사용하지 않는다면

```java
public int MethodName(int a, int b) {
    return a + b;
}
```
으로 표현할 수 있습니다. 


## 함수형 인터페이스 (Functional Interface)

람다의 표현식을 알았다면 다음은 **함수형 인터페이스이다.**  
함수형 인터페이스는 단 하나의 **추상 메소드(abstract method)** 를 가지고 있으며 람다식을 사용하기 위한 명칭이다. 

예를 들어, 아래와 같이 Study 인터페이스에 추상 메소드가 2개 이상이 되어버리면 함수형 인터페이스가 아니다. 

```java
interface Study {
    void addStudy(String studyName);
    void removeStudy(String studyName);
}
```

하지만, Study 인터페이스가 아래와 같이 있으면 
```java
interface Study {
    void addStudy(String studyName);
}
```

이는 함수형 인터페이스이다.  

이전에는 인터페이스를 구현하기 위해서는 클래스를 만들거나, 일회성일 경우 익명 객체를 사용하였습니다.

```java
// 클래스 생성
class StudyInfo implements Study {
    @Override
    public void study(String name) {
        System.out.println("스터디그룹 이름은 " + name + " 입니다. ");
    }
}

// 익명 객체 생성 
Study study = new Study() {
    @Override
    public void addStudy(String name) {
        System.out.println("스터디그룹 이름은 " + name + " 입니다. ");
    }
};
```

위의 코드를 람다식으로 바꾸어 사용하여 구현하고자 한다면

```java
// 람다 특성 적용 전 
Study study = (name) -> System.out.println("스터디그룹 이름은 " + name + " 입니다. ");

// 적용 후, 매개변수가 하나이므로 ( ) 생략 가능
Study study = name -> System.out.println("스터디그룹 이름은 " + name + " 입니다. ");

study.addStudy("알고리즘");

// 결과값 : 스터디그룹 이름은 알고리즘 입니다. 
```

위와 같은 코드가 될 것입니다. 

이처럼 익명 객체를 람다식으로 바꿔 사용할 수 있는 이유는 람다식도 익명 객체이며, 매개변수의 타입과 갯수, 리턴값이 일치하기 때문입니다.

그런데 여러명이서 함께 이 코드를 관리한다고 할 때 만약 다른 사람이 Study 인터페이스를 수정하려 한다면 어떻게 해야 할까요?!
다른 곳에 인터페이스를 사용하는 람다식이 있다는 걸 모르고 말이죠. 에러가 나게 되겠죠?? 

이런 일을 막기 위해서 우리는 사용할 수 있는 것이 있습니다. 바로 애노테이션입니다. **@FunctionalInterface 애노테이션** 을 붙여주면 해당 인터페이스가 함수형 인터페이스인지 컴파일러가 확인해줍니다.

```java
@FunctionInterface
interface Study {
    void addStudy(String studyName);
}
```

@FunctionalInterface 애노테이션을 꼭 붙여야 하는 것은 아니랍니다! 하지만 함수형 인터페이스라는 것을 알리기 위함이니 붙이는게 좋을 듯 싶습니다!

또한 람다식으로 구현할 때 객체는 **상태를 가질 수 없습니다.** 이게 무슨 말일까요?  
익명 객체에서는 아래와 같이 studyName 이라는 필드를 가질 수 있지만 람다에서는 바로 식으로 표현되니 필드가 들어갈 수 없습니다. 

```java
Study study = new Study() {
    private int studyName;
    
    @Override
    public void addStudy(String name) {
        System.out.println("스터디그룹 이름은 " + studyName + " 입니다. ");
    }
};
```

위에서 익명 객체는 객체에 종속되어 있기 때문에 Input에 상관없이 얼마든지 객체의 상태에 따라 결과값이 달라질 수 있습니다. 
하지만 위에서 람다는 함수형 프로그래밍을 위해 메소드를 하나의 식으로 표현한 것이라고 했습니다. 중요한 것은 함수에서는, Input에 의해서만 Output이 정해진다는 것입니다. 그래서 람다식은 상태를 가질 수 없습니다! 



## 함수형 인터페이스 API
@FunctionalInterface 애노테이션을 이용해서 함수형 인터페이스를 직접 만들어서 사용할 수 있지만, Java8 에서는 몇 가지 기본적인 api를 제공해준다. 

**java.util.function** 패키지에 있으며

| 함수형 인터페이스 | 메소드            | 설명                                                                                    |
|-------------------|-------------------|-----------------------------------------------------------------------------------------|
| Runnable          | void run()        | 매개변수 X, 반환값 X (실행할 수 있는 인터페이스)                                        |
| Supplier          | T get             | 매개변수 X, 반환값 O (제공할 수 있는 인터페이스)                                        |
| Consumer          | void accept(T t)  | 매개변수 O, 반환값 X (소비할 수 있는 인터페이스)                                        |
| Function<T, R>    | R apply(T t)      | 일반적 함수. 하나의 매개변수를 받아 결과를 반환 (입력을 받아 출력할 수 있는 인터페이스) |
| Predicate         | boolean test(T t) | 조건식을 표현. 참 거짓 판단할 수 잇는 인터페이스                                        |
| UnaryOperator     | T apply(T t)      | 단항 연산할 수 있는 인터페이스          

등의 함수들이 있다. 


예를 들어, StudyGroup 클래스가 있다고 한다면 (Predicate 사용)
```
// 특정 Predicate를 사용해 특정 조건에 따라 스터디 그룹 리스트를 가져오는 메소드 
static List<StudyGroup> getStudyGroupList(List<StudyGroup> groups, Predicate<StudyGroup> predicate){
    List<StudyGroup> resultList = new ArrayList<>();
    for(StudyGroup group : groups){
        if(predicate.test(fruit)){
            resultList.add(group);
        }
    }

    return resultList;
}

// 호출하는 부분 
List<StudyGroup> groups = Arrays.asList(new StudyGroup("Algorithm", 10), new StudyGroup("Network", 20), new StudyGroup("Kotlin", 30));
List<StudyGroup> kotlinGroup = getStudyGroupList(groups, new Predicate<StudyGroup>() {
    @Override
    public boolean test(StudyGroup group) {
        return "Kotlin".equals(group.getName());
    }
});


// 위에 호출하는 부분을 간단하게 람다식으로 표현할 수 있다. 
List<StudyGroup> kotlinGroup = getStudyGroupList(groups, group -> "Kotlin".equals(group.getName());

```


## 메소드 참조
람다식이 하나의 메소드만 호출하는 경우, **메소드 참조** 를 통해 람다식을 간단히 할 수 있다.
클래스명::메소드명 or 참조변수::메소드명

```java
// 메소드 참조 적용 전
studyGroupList.forEach(group -> System.out.println(group));

// 메소드 참조 적용 후
studyGroupList.forEach(System.out::println);

// 생성자 참조 
Supplier<StudyGroupClass> s = () -> new StudyGroupClass();  // 람다식
Supplier<StudyGroupClass> s = StudyGroupClass::new; // 메서드 참조
```

메소드 참조가 어떻게 적용된 것일까? 위 코드를 보면  
ArrayList.forEach -> Consumer.appect -> System.out.println 의 콜스택을 가지고 있다.
Consumer은 매개변수는 있고 반환값은 없는 소비할 수 있는 인터페이스이므로 필요가 없다. 따라서 메소드 참조에 의해 변하게 되는 것이다. 



