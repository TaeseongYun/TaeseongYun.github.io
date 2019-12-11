---
layout: post
title: "RecyclerView Adapter 코드 간결화"
date: 2019-12-11 11:51:20 +0900
description: RecyclerView Adapter 코드 간결화 진행과정 # Add post description (optional)
img: refactoring.jpg # Add image post (optional)
---

이번 포스팅은 안드로이드 RecyclerView 제작중에 어떻게하면 RecyclerView Adapter를 간결하게 구현할 수 있을까에 대한 내용입니다.
코드를 개발중에 참고한 사이트가 있는데 포스팅 제일 하단에 참고 사이트를 적어두겠습니다.

## RecyclerView.Adapter

안드로이드를 계속 개발해왔던 개발자라면 아마 자주 사용하는 클래스중 하나 일것이다. 또한 RecyclerView.Adapter를 상속하게 되면 다음과 같은 무수한 override 함수들이 생겨난다.
해당 아래코드는 여태 내가 adapter를 만드는 방식을 가져온 것이다.

{% highlight kotlin %}
class SearchUserAdapter(val context: Context?) : RecyclerView.Adapter<RecyclerView.ViewHolder>(),
BaseRecyclerModel<Item> {
override fun notifyDataItems() {
notifyDataSetChanged()
}

    override fun clearItem() = list.clear()


    val list = mutableListOf<Item>()


    override lateinit var onClick: (Int) -> Unit

    override fun onCreateViewHolder(parent: ViewGroup, ppsition: Int): RecyclerView.ViewHolder =
        SearchUserHolder(onClick, context!!, parent)


    override fun getItemCount(): Int = list.size

    override fun onBindViewHolder(holder: RecyclerView.ViewHolder, position: Int) {
        (holder as SearchUserHolder).onBind(list[position])
    }

    override fun addItem(items: Item) {
        list.add(items)
    }

    override fun getItemData(position: Int): Item = list[position]

}
{% endhighlight %}

위와같은 adapter들을 수도없이 RecyclerView를 UI에 올릴 때 마다 클래스를 생성하는 형식으로 개발 했었다. 매번 adpater를 만들어 주는게 너무 비효율적이라 생각하고 기본 베이스를 바탕으로 분기에 따라 holder를 다르게 주는 방식으로 코드를 리펙토링 해 나가기 시작했다.

## BaseViewHolder

첫 시작은 BaseViewHolder를 제작하였다. RecyclerView.ViewHolder를 상속받은 abstract 클래스 즉 추상클래스 ! 내부에는 바인딩 할 때 자주 쓰이는 View.onBind(확장함수)와 onBind함수를 구현 해 주었다,

{% highlight kotlin %}
@Suppress("UNCHECKED_CAST")
abstract class BaseRecyclerViewHolder<in T: Any?>(
context: Context?,
parent: ViewGroup, @LayoutRes layout: Int
) : RecyclerView.ViewHolder(
LayoutInflater.from(context).inflate(layout, parent, false)
) {
abstract fun View.onBind(item: T)

    fun onBind(item: Any?) {
        itemView.onBind(item as T)
    }

}
{% endhighlight %}

## BaseRecyclerAdapter

그 다음은 RecyclerView.Adapter<VM>을 상속받는 클래스 내부에서 ViewHolder 와 따로 구현 해 주었던 인터페이스를 상속 받기때문에 BaseAdapter에서 오버라이드 해 준다.
BaseRecyclerAdapter는 제네릭이 존재한다.

{% highlight kotlin %}
@Suppress("UNCHECKED_CAST")
abstract class BaseRecyclerAdapter<in T>
: RecyclerView.Adapter<BaseRecyclerViewHolder<MovieResult>>(),
MovieRecyclerModel {

    private val list = mutableListOf<T>()

    override fun notifiedChangedItem() {
        notifyDataSetChanged()
    }

    override lateinit var onClick: (position: Int) -> Unit

    override fun onBindViewHolder(holder: BaseRecyclerViewHolder<MovieResult>, position: Int) {
        holder.onBind(list[position])
    }

    override fun addItems(item: Any?) {
        list.add(item as T)
    }

    override fun clearItems() {
        list.clear()
    }

    override fun getItem(position: Int): Any? = list[position]

    override fun getItemCount(): Int = list.size

}
{% endhighlight %}

## 적용

이제 모든것이 끝났다. 하나의 adapter 클래스에서 이제 분기별로 viewholder를 다르게 주는것이 가능해졌다.

{% highlight kotlin %}
class MovieRecyclerAdapter(private val viewType: ViewType, val context: Context?) :
BaseRecyclerAdapter<MovieResult>() {

    override fun onCreateViewHolder(
        parent: ViewGroup,
        viewType: Int
    ): BaseRecyclerViewHolder<MovieResult> =
        when (this.viewType) {
            ViewType.NOWPLAYING -> MovieRecyclerHolder(onClick, context, parent)
            ViewType.CARDVIEW -> CardStackRecyclerViewHolder(context, parent)
        }

}
{% endhighlight %}

저는 분기를 다르게 주기위해 ViewType 이라는 enum 클래스를 따로 제작하여 분기 하였습니다.

이상 RecyclerView Adapter 리펙토링 포스팅을 마치겠습니다.

-참고 사이트
링크 : [RecyclerView][recyclerlink] [recyclerLink]: https://thdev.tech/android/2018/01/31/Recycler-Adapter-Distinguish/
