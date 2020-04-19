---
layout: post
title: "React & Redux"
date: 2020-04-18 12:30:20 +0900
description: 리덕스 관련 공부. # Add post description (optional)
img: redux.png # Add image post (optional)
---

## 1-1 Redux소개

- 리덕스란 한 애플리케이션에서 상태관리 수월하게 해주는 가장 많이 쓰이는 라이브러리중 하나입니다. React와 같이 사용되면서 널리 이용되고있습니다.

* 일관성있게 작동하고 다른 환경 (클라이언트, 서버 및 기본)에서 실행되며 테스트하기 쉬운 응용 프로그램을 작성하는 데 도움이됩니다.

- 리덕스에서는 3가지 원칙이 있습니다. 3가지 원칙에 대해 알아보겠습니다.

## 1-2 Redux 3가지 원칙

- 리덕스의 상태값은 단일 저장소 내에 개체트리에 저장됩니다.

  이로인해 별도의 코딩작업 필요없이 서버의 상태를 클라이언트에 직렬화된 상태로 통신을 할 수 있기 때문에 수월하게 앱을 만들 수 있습니다.

- 리덕스의 상태값은 읽기전용입니다.

  리덕스의 상태값을 처음사용하게 되면 state에 직접적으로 접근하여 값을 바꾸는 코드를 주로짜는데 state는 읽기 전용이라 action에 따른 매번 새로운 state을 리턴해줘야한다.

- 상태값은 순수함수에 의해서만 변경되어야 합니다.

  상태값을 변경하는 순수함수를 `리듀서`라고 부릅니다.

## 1-3 action

- 위에 3가지 원칙 중 두 번째에 나와있는 action에 대해 알아보겠습니다.

  action이란 type속성을 가지는 JS의 객체 입니다.
  이 때 type은 고유한 값이어야 합니다.

  ex)
  
  ### 리턴되는값은 전부 action에 해당됍니다.

  ```javascript
  function addTodo({ title, priority }) {
    return { type: "todo/Add", title, priority };
  }
  function removeTodo({ id }) {
    return { type: "todo/REMOVE", id };
  }
  function removeAllTodo() {
    return { type: "todo/REMOVE_ALL" };
  }

  store.dispatch(addTodo({ title: "영화 보기", priority: "high" }));

  store.dispatch(removeTodo({ id: 123 }));

  store.dispatch(removeAllTodo());
  ```

  이 때 type 값은 const로 변수를 따로 만들어서 지정해주는것이 좋습니다.

  ```javascript
  const ADD = 'todo/Add'
  const REMOVE = 'todo/REMOVE'
  const REMOVE_ALL = 'todo/REMOVE_ALL'

  ~~~
  ```
## 1-4 dispatch의 역할?

- redux를 공부 중 가장 많이 듣게되는 함수에는 subscribe, dispatch, getState함수가 있는데 그중 dispatch에 대해 알아보겠습니다.

### dispatch
 - 쉽게 말해서 dispatch를 호출하게 되면 reducer를 호출하게 됍니다. reducer는 두 가지의 매개변수를 받게되는데 하나는 이전상태값 (state)과 디스패치에서 들어온 action값을 매개변수로 받는다 action 값이라하면(새로운 오브젝트) 위에서 예시를 들어놓았습니다.


dispatch ex)

```javascript
const store = createStore()

store.dispath({type: 'LOGIN'})
```


여기선 {type: 'LOGIN'}이 액션이 된다.

### reducer

- 리듀서란 위에 디스패치에서 짧게 설명했지만 2개의 매개변수를 받는 함수.
   
   1. 이전상태의 state

  2. dispatch에서 전달 해주었던 action

ex)

```javascript
const reducer = (state = {}, action) => {
  switch(action.type) {
    case 'LOGIN': {
      newState = state.assign({}, {hello: 'world'})
      return newState
    }
  }
}
```
* 리듀서에서 가장 중요한점은 리듀서에서 state를 리턴시킬 때 매개변수로 받은 state를 리턴 시키지 말고 복제본 (assign or ...(스프레드 연산자))로 복제 후 복제한 state를 리턴시켜야 합니다.