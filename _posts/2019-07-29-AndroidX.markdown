---
title: "AndroidX란?"
layout: post
date: 2019-07-29 14:39
image: /assets/images/markdown.jpg
headerImage: false
tag:
- android
category: blog
author: kyudongPark
description: about AndroidX
---

## Intro

이 글에서는 AndroidX에 대해서 알아보겠습니다.

[AndroidX](https://developer.android.com/jetpack/androidx){: target="_blank"} 를 참고하여 작성하였습니다. 

## AndroidX 란?

Android developer 사이트에 들어가보면 이렇게 나와 있습니다. 

> _AndroidX is the open-source project that the Android team uses to develop, test, package, version and release libraries within Jetpack._

이 말은 **AndroidX는 Android 팀이 Jetpack 내에서 라이브러리를 개발, 테스트, 패키지화, 버전 및 릴리스하기 위해 사용하는 오픈 소스 프로젝트입니다.** 라는 말입니다. 
안드로이드 라이브러리 28.0.0 이전에 사용하던 com.android.support.* 또는 android.support.*, android.arch.* 의 패키지명을 androidx.* 으로 변경하였습니다. Android Jetpack 으로 통합하여 제공됩니다. 

사용하는 방법은 생각보다 엄청 간단하고 기존의 프로젝트를 androidX로 변경하는 것도 어렵지 않습니다!!

## 사용법
사용하는 방법은 간단합니다.

> gradle.properties 파일에서

```java
"android.useAndroidX=true"
"android.enableJetifier=true"
```

를 추가해주면 됩니다! 엄청 간단하죠?!

또한 프로젝트 생성 시 Android 9.0 (API 28) 이상으로 컴파일 sdk를 설정하고 androidX 사용을 체크하면 자동으로 생성이 됩니다.

또한 기존의 프로젝트를 androidX으로 이전하고 싶으신 분들은 

<img width="281" alt="androidX_refac" src="https://user-images.githubusercontent.com/18525504/62025752-0ab16a00-b214-11e9-8b36-a407966f805c.png">

**Migrate to AndroidX...** 를 클릭하시면 되네요!! 엄청 쉽네요! 







