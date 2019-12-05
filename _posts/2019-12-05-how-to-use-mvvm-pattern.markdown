---
layout: post
title: "안드로이드 MVVM 패턴"
date: 2019-12-05 21:36:20 GMT+0900
description: 안드로이드 aac를 활용한 MVVM 패턴에 대한 설명 # Add post description (optional)
img: MVVM.png # Add image post (optional)
---

매우 잘 설계된 아키텍처가 개발자에게 주는 이점은 정말 다양하다 그중 가장 큰 이점인 유지보수, 테스트를 용이하게 해준다. 지금부터 마이크로사에서 제안한 MVVM 패턴에 대해서 알아보려한다.

## 안드로이드 MVVM 패턴이란?

![MVVM패턴](../assets/img/MVVM.png)
안드로이드 뿐만아니라 다른곳에서 이용되는 MVVM 패턴은 Model-View-ViewModel 의 약자이다. View는 영어 그대로 UI를 의미하고 ViewModeld은 이벤트처리, Model과의 관계 Model은 비즈니스 로직을 담당한다. 여기서 안드로이드 이전 패턴들을 살펴보면 MVC, MVP 가 있는데 마치 이것은 이번 MVVM도 VM(ViewModel)이 중요하게 생각하도록 만드는데 VM이 아니라 DataBinding에 강점을 두고있다.

## DataBinding?

DataBinding 을 사용하기 위해선 xml 파일에 layout이라는 태그를 최상단에 추가 해 준다
예시 코드는 다음과 같다

{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>

<layout xmlns:android="http://schemas.android.com/apk/res/android"
    >

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <!--
            ...
        -->

    </LinearLayout>

</layout>
{% endhighlight %}

그 다음 Activity 에서 항상해주던 setContentView() 대신에 DataBindingUtil.setContentView()를 이용해서 layout xml을 이어준다.

{% highlight kotlin %}
val binding : PlainActivityBinding =
DataBindingUtil.setContentView(this, R.layout.plain_activity)

#사용 방법
binding.name = "Your name"
binding.lastName = "Your last name"

{% endhighlight %}

**Binding 클래스 이름의 생성은 파스칼 표기법 기준으로 변경된다.
예를 들어 super_main_activity.xml의 파일은 SuperMainActivityBinding 클래스를 생성한다.**

dataBinding에서 변수를 생성하는것은 다음과 같다

{% highlight xml %}
<data>
<variable name="name" type="String"/>
<variable name="lastName" type="String"/>
</data>
{% endhighlight %}

이상 MVVM에 대한 포스트를 마치겠습니다.
