---
layout: post
title: "RoomDatabase 적용"
date: 2019-12-19 12:51:20 +0900
description: RoomDatabase 적용기 # Add post description (optional)
img: room-database.jpeg # Add image post (optional)
---

## Room Database?

Room Database 대해여 자세히 알아가기 위해 android developer 홈페이지를 들어가 보았다.

Room Database는 기존에 안드로이드에 있던 SQLite와는 다르게 어노테이션을 적용하여 SQL문을 조금 더 간편하게 조정할 수 있는 내장 데이터베이스라고 보면 쉽다.

@Insert, @Update, @Delete 등이 존재. Room Database를 사용하려면 총 3가지가 필요로하다. 엔테티, DAO, RoomDatabase를 상속받은 추상화 클래스.

각각의 생김새는 이러하다.

{% highlight kotlin %}

#Entity
@Parcelize
@Entity(primaryKeys = ["id"])
data class MovieResult(
val adult: Boolean,
val backdrop_path: String?,
val genre_ids: List<Int>,
val id: Int,
@SerializedName("original_language")
val originalLanguage: String,
@SerializedName("original_title")
val originalTitle: String,
val overview: String,
val popularity: Double,
@SerializedName("poster_path")
val posterPath: String,
val release_date: String,
val title: String,
val video: Boolean,
val vote_average: Double,
val vote_count: Int,
var isLike: Boolean
): Parcelable

{% endhighlight %}

위에는 또 내가 처음 사용하게 된 Parcelable 이라는 클래스와 @Parcelize라는 어노테이션이 존재한다. Parcelable의 클래스는 안드로이드 개발자가 직렬화 하기 위해 사용하는 하나의 클래스라고 한다.
참고는 [https://www.3pillarglobal.com/insights/parcelable-vs-java-serialization-in-android-app-development](https://www.3pillarglobal.com/insights/parcelable-vs-java-serialization-in-android-app-development) 여기서 하였다.

속도면의 차이에서 Serialization 보다 Parcelable가 훨씬 빠르다고 한다. 하지만 Parcelable은 굉장히 불편하다고 한다.
해당 불편함을 줄이고자 @Parcelize 어노테이션이 생겨났다. Parcelable을 상속한 데이터클래스에 @Parcelize를 붙혀주면 writeToParcel()/createFromParcel()를 자동으로 구현해준다.

### 설명글

An automatic Parcelable implementation generator. Declare the serialized properties in a primary constructor and add a @Parcelize annotation, and writeToParcel()/createFromParcel() methods will be created automatically

## RoomDatabaseConverter???

언제부터인가 Room 데이터베이스를 사용하다보면 계속해서 컨버터 오류가 났었다.
![컨버터오류](../assets/img/convertError.png)
여러 페이지에 검색을 해보니 해당 오류는 일반적인 오류라고 한다.

Room 데이터베이스는 직접 저장하는기능에서 변환 기능이 따로 존재하지 않기때문에 List같은 경우는 List 제네릭 형에 맞춰서 컨버터를 제작해야한다고 나와있었다.

참고하여 만들어준 컨버터 클래스는 다음과 같다.
![컨버터 클래스](../assets/img/IntegerConvert.png)

해당 룸 컨버터를 자세히 알아가기 위해 Android 디벨로퍼 홈페이지와 TypeToken 이 어떤 클래스인지 알아보았다.

컨버터를 작성하기 위해선 처음엔 어노테이션인 @TypeConverter를 붙혀줘야하고,

Gson(구글에서 제공하는 json 변환(직렬화, 역직렬화) 클래스)의 type

즉 변환해야 할 타입의 토큰을 알아야한다. 정확한 이유가 맞는진 모르겠지만 type 은 TypeToken 제네릭에서 구분하는 것으로 보인다.

요약

- Room 데이터베이스는 기존에 SQLite 보다 구현하기 쉽도록 Jetpack에 잘 나와있고 어노테이션으로 @Entity, @Dao, @Query, @Insert, @Delete 등 사용 가능하다.
- Room 으로 기존의 간단한 String, int, float 등 기존 타입은 가능하지만 Parcelize형의 클래스는 따로 컨버터가 있어야 한다. @TypeConverter 어노테이션으로 지정.

참고자료

- [https://www.3pillarglobal.com/insights/parcelable-vs-java-serialization-in-android-app-development](https://www.3pillarglobal.com/insights/parcelable-vs-java-serialization-in-android-app-development)
- [https://proandroiddev.com/parcelable-in-kotlin-here-comes-parcelize-b998d5a5fcac](https://proandroiddev.com/parcelable-in-kotlin-here-comes-parcelize-b998d5a5fcac)
- [https://zerodice0.tistory.com/127](https://zerodice0.tistory.com/127)
