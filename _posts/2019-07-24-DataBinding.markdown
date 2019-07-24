---
title: "Data Binding"
layout: post
date: 2019-07-24 22:48
image: /assets/images/markdown.jpg
headerImage: false
tag:
- kotlin
- databinding
- android
category: blog
author: kyudongPark
description: Easy example of data binding
---

## Intro

이 글에서는 안드로이드 개발을 할 때 사용하는 데이터 바인딩이 무엇인지, 장단점과 간단하게 사용하는 방법을 함께 알아보겠습니다

## Data Binding 이란?

Android developer 사이트에 들어가보면 이렇게 나와 있습니다. "The Data Binding Library is a support library that allows you to bind UI components in your layouts to data sources in your app using a declarative format rather than programmatically". 즉, 레이아웃에 있는 UI 컴포넌트를 데이터 소스에 바인딩 할 수 있는 서포트 라이브러리입니다.
생각보다 어렵지 않은 개념인 것 같습니다! 더 쉽게 이해하기 위해 코드를 보겠습니다. 안드로이드 개발을 할 때 TextView, Button, EditText 등의 UI를 나타내는 위젯을 많이 사용합니다. 그리고..!! 코드에서 UI가 복잡해질수록 많이 사용하게 되는 **findViewById()..!!!!**

```java
var textView = findViewById<TextView>(R.id.firstNameTxv)
textView.setText("FirstName")
```

이 코드가


아무것도 하지 않아도 되는 코드가 됩니다!!!
