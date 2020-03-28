---
layout: post
title: "What is livedata?"
date: 2020-01-19 12:51:20 +0900
description: LiveData와 object 키워드 # Add post description (optional)
img: livedata.png # Add image post (optional)
---

## LiveData란 무엇인가??

- LiveData라 하면 관찰주기가 있는 데이터 홀더 클래스이다. 일반 관찰기능과는 달리 라이브데이터는 생명주기를 관찰하기 때문에 라이브데이터의 생명주기가
  STATED, RESUME 일 때 해당 데이터 변화에따라 감지 합니다. DESTROYED 되었을 때는 더이상 데이터를 감자하지 않는다는것이기때문에 액티비티와 프레그먼트의 라이프사이클이 파괴될 때 그 즉시 구독을 취소합니다.

## LiveData의 장점

- UI 데이터가 일치하는지 확인.

- 항상 최신 데이터

- 활동 중지로 인한 충돌 없음.

- 메모리 누수 없음.

## LiveData를 이용하다가 알게된 점.

- LiveData는 백그라운드로 들어가게 되면 그 즉시 Livedata는 일시정지 하게 된다. 따라서 rxjava를 이용해서 LiveData를 사용하려면 메인스레드(UI스레드)에서 돌려줘야한다.

## MutableLiveData and LiveData

- 라이브 데이터를 사용하다 보면 MutableLiveData와 LiveData 이렇게 두가지가 주로 쓰인다.

  MutableLiveData는 LiveData를 상속받고 있는 클래스이다. 만약 LiveData의 데이터를 변경하고 싶으면 MutableLiveData를 사용하면된다. LiveData에는 데이터를 변경할 수 있는 메소드가 퍼블릭하게 있지 않다.

  그러면 MutableLiveData를 이용해서 데이터를 변경해야 하는데 변경 방법에는 또 두가지가 있다. setValue와 postValue 이렇게 두 가지가 있는데 setValue는 반드시 mainThread에서 작업이 이루어져야 한다. 안그러면 앱이 터지고 만다.(강제 종료를 뜻함.)

  처음 사이드 프로젝트 진행 할 때 무조건 Main Thread에서 작업 해야하는 줄 알았는데 잘못된 개념 이해였다.

## object 키워드

- 코틀린에서 object를 사용할 때 마다 헷갈리는 경우가 종종생겨 바로잡기 위해 다시 포스팅 하게되었다.

코틀린 object 키워드를 사용하게 되면 선언과 동시에 객체를 만들게 된다. 그리고 object는 생성자를 사용하지 못한다. object를 사용한 클래스에 디코딩을 하게되면 object의 생성자는
private으로 되어 나오기 때문이다.

## ViewModelProvider get함수?
