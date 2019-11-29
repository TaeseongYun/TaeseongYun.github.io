---
layout: post
title: "Django Rest Framework 사용기"
date: 2019-11-30 01:47:20 GMT+0900
description: Django에 대한 설명 # Add post description (optional)
img: REST-frame-work.png # Add image post (optional)
---

# What's Django-Rest-Framework??

Django-Rest-Framework란 Django 안에서 RESTapi 서버를 쉽게 구축할 수 있도록 도와주는 오픈 소스 라이브러리이다.

## 그럼 Django란?

![Django](../assets/img/django.jpeg)

Django(장고)는 파이썬으로 만들어진 무료 오픈소스 웹 어플리케이션 프레임워크이고 쉽고 빠르게 웹 사이트를 개발할 수 있도록 돕는 구성요소로 이루어져있다.

## 설치방법

python은 한 프로젝트에 한 가상환경을 해주어야 후에 충돌이 일어나는 경우가 매우 적어지기 때문에 가상환경에서 실행해준다. 다음 설치 방법은 맥os 기준이다.

{% highlight python %} #우선 가상환경을 설치 해준다.
pip3 install virtualenv

#그 후 가상환경을 설정해줘야 한다.
python -m venv [설정하고싶은 가상환경이름]

#가상환경 실행
source [설정한 가상환경 이름]/bin/activate

#가상환경에서 django 와 djangorestframework를 설치해준다.

pip install django, pip install djangorestframework

#그 후 프로젝트와 앱을 하나 만들어준다.

#[]안에는 각각 설정할 프로젝트명과 설정할 앱 명을 넣는다
django-admin startproject [설정할 프로젝트명]
django-admin startapp [설정할 앱 명]

# 만들어 준 프로젝트 안의 settings.py 파일안의 INSTALLED_APPS에는 'rest_framework'를 추가시키고

INSTALLED_APPS = [
... ,
# 이것 추가
'rest_framework',
... ,
]

#요건 settings.py파일 제일 밑에 추가시켜준다.
REST_FRAMEWORK = {
'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
'PAGE_SIZE': 10
}
{% endhighlight %}

drf(Django-rest-framework)하면서 제일 헷갈렸던 부분이 urls 부븐인데 만들어줬던 앱 안에서는 urls.py파일을 새로 만들어주고 project에 include 시켜줘야한다.

프로젝트의 urls.py는 다음과 같다.

{% highlight python %}
from django.contrib import admin
from django.urls import path, include

urlpatterns = [

    # 최초 경로
    path('admin/', admin.site.urls),

    # 추가 해주는 경로 include는 내가 아는 include 와 같음
    path('', include('dormMealsProject.dormMealList.urls'))

]
{% endhighlight %}

또 생성했던 앱 파일안의 urls.py은 다음과같다.

{% highlight python %}
from django.urls import path
from .views import DormMealView

urlpatterns = [
path('', DormMealView.as_view())
]
{% endhighlight %}

위에 보이는것은 Class가 바탕이 된 view 형식이다.

## Selenium이란?

동적으로 생성된 페이지를 크롤링 할때 유용하게 사용되는 스크래핑 도구 이번에 사용한것은 Selenium과 Beautiful soup 두가지를 이용하였다.

두개를 이용한 이유는 크롤링 할 홈페이지가 iframe안에 감싸져 있기때문에 뷰티풀수프(BeautifulSoup)만으로는 크롤링이 불가능했기 때문에 셀레니움으로 띄운 홈페이지에서 뷰티풀수프를 사용하여 크롤링을 해주었다.

{% highlight python %}
pip install bs4, pip install selenium

#크롬 드라이버 있는 곳
driver = webdriver.Chrome('자신의 크롬드라이버 있는 위치') #크롤링 할 홈페이지 주소
driver.get('https://busan.happydorm.or.kr/')
#frame을 스위치 해줌
driver.switch_to.frame(driver.find_element_by_tag_name('frame'))

#셀리니움으로 받은 페이지 소스를 뷰티풀수프에 넘겨줌
happy_bs = BeautifulSoup(driver.page_source, 'html.parser')

#긁어오고싶은 태그 긁어옴
happy_morning_tag = happy_bs.select('#food > ul.bf_li.fmenu_box')
happy_lunch_tag = happy_bs.select('#food > ul.lu_li.fmenu_box')
happy_dinner_tag = happy_bs.select('#food > ul.dn_li.fmenu_box')
{% endhighlight %}

# Django Views??

내가 사용한 장고 뷰 코드는 다음과 같다.

{% highlight python %}
from rest_framework.response import Response
from rest_framework.views import APIView
from .meals import happy_dormMeal

# Create your views here.

파이썬은 다른언어와 다르게 상속을 클래스 ()안에다 해줌

그러고 happy_dormMeal()은 따로 만들어준 py안의 함수인데 import시켜주었다.
리턴되는값이 총 4개인데 파이썬은 저렇게 가능하다.

class DormMealView(APIView):
def get(self, request):
morning_non_bread, morning, lunch, dinner = happy_dormMeal()
return Response({
'아침(빵x)': morning_non_bread,
'아침(빵o)': morning,
'점심': lunch,
'저녁': dinner
})
{% endhighlight %}

이상 Django 사용 글이었습니다.
