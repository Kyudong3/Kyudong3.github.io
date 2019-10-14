---
title: "[Kotlin] 코틀린의 Scope function"
layout: post
date: 2019-06-30 15:48
image: /assets/images/markdown.jpg
headerImage: false
tag:
- kotlin
- Java
- android
category: blog
author: kyudongPark
description: Difference of also, let, with, apply, run in Kotlin
---

그동안 코틀린을 사용하면서 헷갈렸던 run, with, apply, also, let에 대해서 알아보는 시간을 가지도록 하겠습니다.

자바에는 없지만 코틀린에서는 기본적으로 제공하는 함수 라이브러리들이 있습니다.  
그 중에 run, with, apply, also, let은 Scope Function에 해당됩니다.

왜 scope function을 사용하는 걸까요? **객체에 접근하는 방법을 간단하게 해서 코드를 간결하게 하고 가독성을 높일 수 있기 때문입니다.**
그 전에 scope function을 이해하기 위해서 리시버와 람다함수에 대해 잠깐 소개하도록 하겠습니다.  
Scope function을 사용하면 두 개의 객체를 넘겨주게 되는데 바로 **리시버** 와 **람다함수** 입니다.

```java
Person("KyuDong", 27, "Korea").let {
  println(it)
  it.moveTo("London")
  it.incrementAge()
  println(it)
}
```
이러한 scope function을 적용한 코드가 있다고 가정해봅시다.
간단히 말해서 Person 객체를 리시버라고 하고 let 다음 블록 { ... } 을 람다함수라고 합니다. 
위 예제에서는 람다함수 안에서 리시버를 it의 형태로 접근하고 있네요!

그럼 이제 이 함수들이 각각 어떤 역할을 하는지 알아보겠습니다.

기본적으로 이 함수들은 객체의 코드 블럭을 실행한다는 같은 역할을 가지고 있습니다. 
이들 사이에 가장 중요한 2가지 차이점은 바로
* 블록 안에서 객체에 접근하는 방식
* 전체식의 결과값
입니다.

객체에 접근하는 방식은 **this(람다 리시버)** 와 **it(람다 매개변수)** 이 있습니다.  
이 함수들은 this와 it 중 하나를 사용합니다.  
**run**, **with**, **apply** 는 람다 리시버인 **this** 를 사용합니다.
**let** 과 **also** 는 **it** 을 사용합니다.

또한 **let**, **run**, **with** 는 람다의 결과를 리턴하고 **apply** 와 **also** 는 리시버 객체 자체를 리턴합니다.

예시를 보면서 쉽게 이해 해보도록 하겠습니다.


```java
fun main() {
    val numbers = mutableListOf<String>("one", "two", "three")
    val countEndsWithE = numbers.run {
        this.add("four")
        this.add("five")
        add("six")
        count { it.endsWith("e") }
    }
    println("return value is $countEndsWithE")
}

// 실행결과 : return value is 3
```

리스트에 run 함수를 통해 람다함수 안에서 값을 추가해주고 "e"로 끝나는 단어를 카운트해 주는 코드입니다.  
코드를 보면 **run** 을 사용하기 때문에 람다함수 안에서 **this** 를 통해 객체에 접근하는 것을 볼 수 있습니다. **this** 를 생략하여 사용할 수도 있습니다. 또한 마지막 줄의 값을 리턴한다는 것을 알 수 있습니다. "e"로 끝나는 단어는 one, three, five 3개이므로 3을 리턴하게 됩니다.  
만약 add("six")와 count 줄을 바꾸게 된다면 결과는 어떻게 될까요?

```java
실행결과 : return value is true
```

가 됩니다.

또 다른 예제 하나를 보도록 하겠습니다.

```java
fun main() {
    val numberList = mutableListOf<Double>()
    numberList.also { println("Populating the list") }
        .apply {
            add(2.71)
            add(3.14)
            add(1.0)
        }
        .also { println("Sorting the list") }
        .sort()
}
```

리스트에 Double 값을 넣고 **also** 와 **apply** 함수를 사용하고 있습니다. **also** 와 **apply** 는 객체 자체를 리턴하기 때문에 
마지막 줄의 sort() 메소드를 부를 수 있습니다. 

이제 좀 감이 오시나요? 

그렇다면 이제부터는 각 함수가 어떠한 특징을 가지고 어떤 상황에서 사용을 하는지 살펴봅시다.

## let
**let** 은 **it** 를 사용하여 객체에 접근하고 람다의 결과를 리턴합니다.  

**let** 을 사용하는 경우는 크게 3가지 입니다.  
* Null check 하기 위해
* 가독성을 위해 지역 변수의 범위를 제한하기 위해
* 블록 안의 결과를 사용하기 위해 

```java
fun main() {
    val numbers: List<String>? = listOf("one", "two", "three", "four")
    val modifiedFirstItem = numbers?.first()?.let { firstItem ->
        println("The first item of the list is '$firstItem'")
        if (firstItem.length >= 5) firstItem else "!" + firstItem + "!"
    }?.toUpperCase()
    println("First item after modifications: '$modifiedFirstItem'") 
}
```

위 코드를 보면 Nullable 타입 리스트에 let 함수를 사용할 때 Null check를 해주고 또한 지역 변수 **firstItem** 을 블록 안에서만 사용하도록 하였습니다. 


## with
**with** 는 **this** 를 사용하여 객체에 접근하고 람다의 결과를 리턴합니다.

**with** 는 let과 다르게 비확장 함수입니다. **with(object)** 형태로 사용하며
* 람다의 결과값을 사용하지 않을 때
* 속성이나 함수가 값을 계산할 때의 Helper 객체로 
사용하는 것을 권장하고 있습니다. 

```java
fun main() {
    val numbers = mutableListOf("one", "two", "three")
    with(numbers) {
        println("'with' is called with argument $this")
        println("It contains $size elements")
    }
}

실행결과 : 'with' is called with argument [one, two, three]
         It contains 3 elements
```

위의 코드를 보면 값을 출력만 하고 결과값을 따로 사용하고 있지 않습니다. 

## run
**run** 은 **this** 를 사용하여 객체에 접근하고 람다의 결과를 리턴합니다. **run** 은 **with** 와 같은 특징을 가지고 있지만 **let** 처럼 확장함수입니다. 

**run** 은 
* 객체 초기화와 리턴 값을 초기화에 사용할 때
* 이미 생성된 객체에 연속된 작업이 필요할 때
주로 사용합니다.


```java
fun main() {
    val service = MultiportService("https://example.kotlinlang.org", 80)
    val result = service.run {
        port = 8080
        query(prepareRequest() + " to port $port")
    }
}
```

위 코드를 보면 이미 생성된 service라는 객체에 연속된 작업으로 객체를 초기화하는 것을 볼 수 있습니다.  
위에서 run은 확장함수라고 했지만, **run** 은 **non-extension** 함수로도 사용할 수 있습니다. **val num = run { }** 이런 식으로 말이죠.

## apply
**apply** 는 **this** 를 사용하여 객체에 접근하고 리시버 객체 자체를 리턴합니다.

**apply** 는 
* 값을 리턴하지 않거나
* 리시버 객체의 멤버에서 작동할 때 
* 객체 생성과 동시에 초기화 할 때
주로 사용합니다. 

```java
fun main() {
    val adam = Person("Adam").apply {
    age = 32
    city = "London"        
    }
}
```

위 코드를 보면 리시버인 **Person** 객체가 있습니다. **Adam** 이라는 객체는 **age = 32 , city = London** 의 멤버를 가지고 이 객체를 반환하는 것을 볼 수 있습니다.

## also
마지막으로 **also** 는 **it** 를 사용하여 객체에 접근하고 리시버 객체 자체를 리턴합니다.

**also** 는 
* logging이나 debug정보를 프린트 하는 것처럼 객체 정보를 바꾸지 않을 때 
주로 사용합니다. 

```java
fun main() {
    val numbers = mutableListOf("one", "two", "three")
    numbers.also { println("The list elements before adding new one: $it") }
           .add("four")
}
```

위 코드를 보면 리비서 객체 numbers의 **also** 의 블록 안에는 단순히 내용을 출력하는 것을 볼 수 있습니다.


## 결론
처음에는 헷갈리지만 공부하고 코드에 적용해보면 생각보다 자주 사용하게 되는 패턴들이 있다는 것을 알 수 있습니다!  

혹시나 틀린 게 있다면 댓글 남겨주시면 감사하겠습니다 :)


