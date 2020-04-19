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
