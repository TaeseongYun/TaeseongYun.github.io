---
layout: post
title: "Koin 프로젝트 적용"
date: 2019-12-30 12:51:20 +0900
description: Koin 프로젝트 적용 # Add post description (optional)
img: koin.png # Add image post (optional)
---

## Koin 정리

의존성 주입 중 하나인 (DI) Koin에 대해 정리한것을 포스팅 해보려고 합니다. Dagger가 DI 중 가장 많이 쓰이는 프레임워크이지만 학부생인 저에겐 러닝커브가 높은것처럼 느껴져
그나마 러닝커브가 낮은 Koin을 현재 토이프로젝트로 하고있는곳에 적용을 시켜보고 후에 하나하나씩 Dagger를 적용해보려고 합니다.

## DI란 무엇인가?

우선 Koin에 대해 자세히 들어가기전에 DI란 무엇인지 알아보고 가려고 합니다. DI란 영어로 풀어쓰면 `Dependency Injection` 해당 영어를 변역을 해보면
Dependency -> 의존성 Injection -> 주입 말 그대로 의존성 주입입니다.
의존성이라 함은 예를들어 B라는 클래스에 A라는 객체가 필요하다면 다음과 같이 해야합니다.

{% highlight kotlin %}
// 새로 추가된 클래스
class C {
...
}

class B(private val c: C) {
...
}

class A {

val b = B(C()) // <- B클래스가 변경됨으로 이 부분에 변화가 생깁니다.
...

}
{% endhighlight %}

보시는것처럼 A클래스안에서 b의 객체를 만드는데에는 C라는 클래스가 반드시 필요합니다. 이것을 바로 의존성이라고 합니다.

이 때 DI 를 이용할 수 있는데 DI를 하게되면 다음과 같은 장점이 있습니다.

- 코드가 단순화 된다.
- 결합도를 낮추게된다.
- 테스트에 용이.

다양한 장점이 존재하기에 러닝커브가 비교적 높은 dagger2 보단 그나마 접근하기 쉬운 koin을 택하여 하나하나씩 적용해 나가기로 했습니다.

먼저 적용 했던것은 바로 Room DI적용!

![Room](../assets/img/Room-Database.png)

getInstance함수를 보게되면 context가 필요하다. 다른코드를 참고하며 module를 하나 만들어보았다.

![DI(module)](../assets/img/DI-module.png)

module 이란 inject 하기 전에 어느 모듈을 투입할건지 정하는? 그런 함수이다.

현재 repository, database, retorfit 적용하였지만 viewmodel을 적용하지 못했음.
후에 조금 더 적용 시키고 추가하도록 하겠습니다.
