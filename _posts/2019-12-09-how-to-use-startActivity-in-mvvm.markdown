---
layout: post
title: "MVVM 패턴에서 View와 ViewModel 의존성 해결방안"
date: 2019-12-09 21:51:20 +0900
description: View와 ViewModel 의존성 해결방법 # Add post description (optional)
img: intent.png # Add image post (optional)
---

첫 MVVM 패턴 사용이여서 View 와 ViewModel에 의존성을 없애야하는 방법을 잘 몰랐을 때 startActivity 사용과 해결한 방법을 포스팅 하겠습니다.

## ViewModel startActivity?

처음 ViewModel 특징도 잘 모른체 Presente와 똑같이 View와 ViewModel간의 의존성을 추가시킨채 그냥 무턱대고 함수 만들어서 함수 안에 startActivity를 클릭 이벤트 안에다가 넣었었는데 뭔가 이상하다 싶어서 구글에 검색을 해보니 View와 ViewModel 사이에는 의존성이 없다. 라고 되어있는걸 보고 어떻게 짜면 의존성을 지운채 구현할 수 있을까 많은 생각을 했었습니다.
아래 보이는 사진이 처음 코드를 구현 했을 때 모습입니다.

![스타트 액티비티 함수](../assets/img/startActivityFunction.png)

![클릭 이벤트 함수](../assets/img/clickEvent.png)

## Using Pair!!

LiveData를 이용하여 해당 값이 변경되었을 때 또는 어떤 임의의 값이 아닐 때 Activity안에서 실행되도록 하려고 코드를 짜다보니 Pair라고 Java에는 없고 Kotlin에서 구조 분해 변수 초기화 할 때 사용하는 클래스가 있었습니다.

{% highlight kotlin %}
val(index, value) = 1 to "one"
index => 1
value => "one"
{% endhighlight %}

이렇게 되는 to 함수가 infix 를 이용한 (중위함수) 즉 Pair를 리턴하는 함수. 가 가장 대표적인 Pair가 사용되는 경우입니다.

여기서 Pair를 좀 더 자세히 들여다 보기위해 Pair클래스를 들어가 보았습니다.

![Pair클래스](../assets/img/PairClass.png)

보이는 것과 같이 제네릭으로 쓰기만 가능한 out 태의 A, B 가 두개 존재하고 그렇다는 말은 각기 다른 클래스가 들어올 수 있다는 말.

내부에는 first, second이 존재합니다.

앞서 예시로 들었던 to 함수도 보여집니다. infix 가 중위함수 즉 일반적으로 함수 접근할 때에는 .size()이렇게 접근을 하는데 그럴 필요없이 위의 예처럼 저렇게 접근 가능하게 만드는걸 중위함수 라고 합니다 키워드는 infix!

이상 ViewModel과 View 간의 의존성 해결방안 포스팅을 마치겠습니다.
