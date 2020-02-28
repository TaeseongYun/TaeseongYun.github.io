---
layout: post
title: "How to use android jetpack?"
date: 2020-02-26 12:51:20 +0900
description: 안드로이드 JetPack 사용기 # Add post description (optional)
img: android-jetpack.png # Add image post (optional)
---

## Android JetPack

안드로이드 제트팩이라하면은 안드로이드 개발자가 고품질 어플리케이션을 보다 쉽게 만들게해주는 라이브러리, 도구 모음이다.

여태 내가 사용했던 제트팩은 Room, Databinding, lifecycle, livedata 등이 있지만 이번에 포스팅하게 될 내용은 안드로이드 제트팩 중 하나인 Navigation이다.

## Navigation

안드로이드 네이게이션은 다른 콘텐츠를 탐색하고 다시 탐색할 수 있는 상호작용을 말한다. 즉, 다른 화면으로 이동한다. 라는 뜻이고 안드로이드에 있어 가장 중요한 요소중 하나이다.

이전에는 인텐트나 supportFragmentManagement를 이용해서 화면을 이동했지만 만약 클릭하는게 많아지면? 제공하는 컨텍스트에 따라 이동해야하는 동작이 많아지면? 그만큼 코드가 복잡해질것이고 코드길이는 증가할것이다.

### Navigation Graph

네비게이션 그래프란 xml리소스 파일로 모든 네비게이션 정보가 담겨져 있는 파일이다.
여기에는 destination이라고 불리는 앱 내 모든 개별 콘텐츠 영역과 경로가 포함된다.

### NavHost

탐색 대상을 표시하는 빈 컨테이너를 의미. (나중 그림에서 보겠지만 네비게이션 하나하나가 NavHost?)

공식문서를 확인해보니 <argument>태그는 fragment에 들어가는 태그이고 만약 해당 타입이 Parcelable이면 해당클래스를 넣어줘야하는데 `이 때 주의해야할점이 클래스 명만 넣는것이 아니라 클래스 패스값 즉 패키지명을 그대로 넣어줘야한다`

네이게이션 사용 전 코드 )

{% highlight kotlin %}

movieFragment.setFragment()

bottom_sheet_menu.setupWithNavController(bottom_sheet_menu, R.id.nav_graph)
        bottom_sheet_menu.setOnNavigationItemSelectedListener { menuItem ->
            when (menuItem.itemId) {
                R.id.movie_fragment -> {
                    movieFragment.setFragment()
                    true
                }
                R.id.tv_fragment -> {
                    tvFragment.setFragment()
                    true
                }
                R.id.star_fragment -> {
                    starFragment.setFragment()
                    true
                }

                else -> false
            }
        }

searchViewModel.onShowProgressBar = {
    frameLayout.visibility = View.GONE
    loading_progress.visibility = View.VISIBLE
}

{% endhighlight %}

변경 후 코드)

{% highlight kotlin %}

navFragmentHost = supportFragmentManager.findFragmentById(R.id.nav_host_fragment) as NavHostFragment?

navFragmentHost?.let {
    NavigationUI.setupWithNavController(bottom_sheet_menu, it.navController)
}

{% endhighlight %}

네비게이션 중 가장 중요한것이 NavigationHost `NavigationHost`은 사용자가 앱을 탐색할 때 대상이 바뀌거나 빈 컨테이터를 의미.

NavHostFragment 가 NavigationHostFragment를 의미하는것 같음.