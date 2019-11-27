---
layout: post
title: "MVP 패턴이란"
date: 2019-11-27 11:07:20 +0900
description: MVP패턴에 대한 설명 # Add post description (optional)
img: mvp_pattern.png # Add image post (optional)
---

# MVP 패턴

### MVP 패턴이란?

- 각각 model, view, presenter로 구분 되어있고 view와 presenter는 1:1 관계를 가진다.

## Presenter?

- view에서 전달된 이벤트 처리를 하는 곳이고 view와는 무관한 처리를 한다.

## Google Architecture

- MVP 패턴 중에서도 여러가지 아키텍쳐가 존재하지만 그중 가장 접근성이 쉬운 Google 아키텍쳐를 선정하였다.

- 구글아키텍쳐는 다음과 같다.

![구글 MVP아키텍쳐](../assets/img/google-mvp-architecture.png)

```
View : Contract.View를 상속받아서 구현
Contract : 내부에서는 interface view, interface Presenter로 구분지어서 구현
Presenter : Contract 에서 정의 된 Contract.Presenter를 상속받에서 구현
```

즉 Contract를 코드로보면 이러하다

```Kotlin
interface ExampleContract {
    interface View {
// 뷰에 대한 함수 정의
    }

    interface Presenter {
// 뷰이벤트 처리에 대한 함수 정의
    }
}
```

그리고 Contract.View의 사용법은 이렇다. (해당 코드는 직접 사용한 코드를 들고왔다.)

```Kotlin
class RepoStargazersActivity : BaseActivity(), RepoStarredUserListContract.View {
    override fun loadFailGithubApi() {
        toast("API 문서 호출 오류")
    }

    override fun loadFailGithubApi(message: String) {
        toast(message)
    }

    override fun emptyStargazerUserList() {
        user_stargazer_recycler_view?.visibility = View.GONE
        repo_stargazers_empty_lottie_ani?.visibility = View.VISIBLE
    }

    private val starredUserListRecyclerAdapter: StarredUserListRecyclerAdapter by lazy {
        StarredUserListRecyclerAdapter(this)
    }
    private val repoStarredUserListPresenter: RepoStarredUserListPresenter by lazy {
        RepoStarredUserListPresenter(
            this,
            GithubRepository.getInstance(RetrofitObject.githubAPI),
            starredUserListRecyclerAdapter,
            disposable
        )
    }


    private val onScrollRecyclerViewListener = object : RecyclerView.OnScrollListener() {
        override fun onScrolled(recyclerView: RecyclerView, dx: Int, dy: Int) {
            super.onScrolled(recyclerView, dx, dy)

            val firstItem = (recyclerView.layoutManager as? GridLayoutManager)?.findFirstVisibleItemPosition() ?: 0
            val visibleItemCount = recyclerView.childCount
            val totalItemsCount = starredUserListRecyclerAdapter.itemCount

            if (!repoStarredUserListPresenter.isLoading && totalItemsCount - 1 < (firstItem + visibleItemCount)) {
                println("스크롤 뷰 이벤트 들어옴")
                repoStarredUserListPresenter.loadRepoStarredUserList(
                    intent.getStringExtra("repoStargazersUrl") + "/stargazers"
                )
            }

        }
    }

    override fun onStop() {
        super.onStop()

        user_stargazer_recycler_view?.removeOnScrollListener(onScrollRecyclerViewListener)
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_repo_stargazers)

        repo_stargazers_back_img.setOnClickListener { finish() }
        repo_name.text = intent.getStringExtra("repoName")

        repoStarredUserListPresenter.loadRepoStarredUserList(
            intent.getStringExtra("repoStargazersUrl") + "/stargazers"
        )

        user_stargazer_recycler_view.run {
            adapter = starredUserListRecyclerAdapter
            layoutManager = GridLayoutManager(this.context, 1)
            addOnScrollListener(onScrollRecyclerViewListener)
        }
    }
}
```
