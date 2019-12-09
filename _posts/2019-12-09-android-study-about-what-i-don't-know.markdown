---
layout: post
title: "안드로이드 공부"
date: 2019-12-06 21:51:20 +0900
description: 안드로이드 기본 개념 다시 잡기 # Add post description (optional)
img: Android.jpg # Add image post (optional)
---

안드로이드를 개발하다가 기본개념이 다시 많이 무뎌진거 같아 바로 잡기 위해서 기본 다잡기 & 복습 하기위해 포스팅하였다.

## Android R?

안드로이드 개발 시 항상 R.~ 이렇게 진행되는 코드를 매우 많이 봤다. 해당 R이라는 클래스가 어떤 역할을 하는지 상세하게 알아가기 위해

안드로이드 프로젝트의 External Library 안의 R클래스를 들어가보았다.

!['R 클래스'](../assets/img/2019-12-09-R-Class.png)
다음과 같이 많은 코드가 추가된것을 알 수 있다. 해당 추가된 코드들은 res 파일의 상세 디렉토리 style, string, dimen, color 등 이렇게 구분되어있고 각각 변수는 파일명을 따른다. 그로인해 res안에 임의의 디렉토리를 함부로 생성해서는 안된다.

해당 res 파일안의 디렉토리의 각각 변수는 int형으로 생성된다.

## RecyclerView 와 ListView 차이점

ListView는 BaseAdapter를 상속받은 ArrayAdapter 나 CursorAdapter를 어뎁터로 사용하고 ViewHolder 사용을 권장한다. ViewHolder의 사용을 강제적으로 하지 않기때문에 메모리 문제가 생길수도있다. 단 간단한 리스트형테의 작업을 할 때에는 리스트뷰가 더 효율이 좋다.

그러고 ViewHolder를 만들어주기란 쉽지가 않다.

이유) 간단한 리스트의 경우 리스트뷰와 따로 만들어 준 뷰 홀더로 인해 필요없는 함수를 생성하지 않아도 된다. 리사이클러 뷰는 필요없는 함수를 오버라이드 해줘야한다.(강제적인 뷰 홀더 사용때문) (개인적인 생각)

RecyclerView -> ViewHolder를 강제적으로 사용하게 하고 그로인해서 메모리 누수현상이 없다. 단점) ViewHolder를 강제적으로 만들어야 하고 OnClick리스너를 달아줘야하는 문제가 생김

## LiveData?

MVVM에 사용되는 관찰 가능한 클래스이다. LiveData는 생명주기를 알고 있고 살아있는 생명주기에 따라 데이터 값의 변화만 Observer 로 알아간다. 장점) 메모리누수가 없다. LiveData를 이용하면 액티비티 생명주기에 따라 해당 액티비티가 destroy 되면 데이터도 같이 지워진다.

## MutableLiveData ?

이것도 마찬가지로 MVVM 패턴에 사용되고 LiveData를 상속하는 클래스이다. setValue 와 postValue 두 가지로 나눠 지는데 postValue 는 백그라운드 스레드에서 값을 설정, 메인 스레드는 setValue 에서 값을 변경한다.

mutableLiveData의 접근제한자를 private 로 두는이유 액티비티에서 liveData가 관찰하고 있던 mutable 데이터가 있는데 관찰하던 데이터가 액티비에서 변경되면 이전에 관찰하고있던 데이터를 의미가 없어진다. 보안상 문제가 생기기 때문에 private로 둔다.

(정확한 이유를 확실하게 확실하게 알게되면 다시 포스팅 하겠습니다.)

이상 안드로이드 기본기 잡기 포스팅을 마치겠습니다.
