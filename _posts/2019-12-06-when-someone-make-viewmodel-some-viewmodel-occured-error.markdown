---
layout: post
title: "ViewModel 에러"
date: 2019-12-06 21:51:20 +0900
description: ViewModel 생성 시 일어날 수 있는 에러 # Add post description (optional)
img: android-view-model.jpeg # Add image post (optional)
---

## ViewModel 객체 생성 시 일어날 수 있는 에러

첫 MVVM 패턴을 개인 프로젝트에 적용 시켜야겠다고 생각 한 후 ViewModel을 적용 시키고 객체를 생성 시키는 과정에서 해당 에러가 자꾸 발생 하였다.

![viewmodel-error](../assets/img/viewmodel-error.png)

해당 에러는 컴파일 에러는 아니었지만 런타임에서 계속해서 발생하는 에러라 원인이 어디있는지 알기 힘들었다. 상세한 원인을 알기위해 ViewModelProviders 의 of 함수로 들어가 보았다.

![viewmodel-of-function](../assets/img/viewmodelprovider-of-function.png)

보이는것 처럼 factory가 of 함수 사용할 때 정의되어있지 않으면 ViewModel의 생성자를 전달 해 주지 않는다. ViewModel의 객체를 만들 때 생성자를 이용하고 싶으면 ViewModelProvider 의 Factory 를 이용해야한다.

코드는 다음과 같이 구현 하였다.
{% highlight kotlin %}
@Suppress("UNCHECKED_CAST")
class MovieViewModelFactory(private val movieRepository: MovieRepository) : ViewModelProvider.Factory {
override fun <T : ViewModel> create(modelClass: Class<T>): T {

        return MovieNowPlayingViewModel(movieRepository) as T
    }

}
{% endhighlight %}

위의 코드처럼 펙토리를 따로 생성시켜준다음 액티비티나 프레그먼트에 ViewModelProviders.of(fragmentActivity: FragmentActivity(해당 액티비티나 프래그먼트), 생성했던 팩토리) 이런식으로 넘겨준다.

## ViewModelProviders -> Deprecated !!!

ViewModelProviders는 Deprecated 되었습니다. 그 뜻은 ViewModelProviders로 개발은 가능하나 더 이상 업데이트를 진행하지 않기 때문에 `특정 버전부터는 에러가 날 수 있다 사용을 중지 해야한다.` 는 뜻을 내포하고 있다.

## Use ViewModelProvider !!!

`ViewModelProviders` 가 Deprecated 되었기 떄문에 ViewModelProvider를 사용해야한다.

{% highlight kotlin %}
private lateinit var noParamViewModel: NoParamViewModel

noParamViewModel = ViewModelProvider(this).get(NoParamViewModel::class.java)
{% endhighlight %}

## ViewModelProvider Constructor

ViewModelProvider constructor에 들어가는 this는 ViewModelStoreOwner 타입이다.

[ViewModelStoreOwner 공식문서.](https://developer.android.com/reference/kotlin/androidx/lifecycle/ViewModelStoreOwner?hl=en)

생성자에 첫 번째 매개변수에는 액티비티나 프래그먼트가 들어간다. 두 번째는 ViewModelFactory가 들어가게 되는데 매개변수가 들어가는 ViewModel 일 경우 주로 사용된다.

`create`함수로 매개변수를 넘겨주기 위함.

모든 ViewModel 초기화는 위 포스트 처럼 길게 사용할 수 있지만 조금 더 간결화 된 코드를 쓰고싶다면 공식문서에 있는 ktx 모듈을 사용하게 되면 편리하게 인스턴스를 생성 가능하다.

[안드로이드 ktx 홈페이지](https://developer.android.com/kotlin/ktx?hl=en#fragment)

이상 ViewModel 생성자에러에 대한 포스트를 마치겠습니다.
