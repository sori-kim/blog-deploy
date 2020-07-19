---
title: '[React Basis] Component Life Cycle API'
date: 2020-05-07 04:21:13
category: 'react'
draft: false
---

## Component Life Cycle

앞서 살펴 봤던 Class Component에서 가장 많이 사용되는 method는 render()지만 사실 react component는 단순히 render 말고 더 많은 걸 갖고 있다.
모든 Componenent들은 `life cycle method`를 가지는데, life cycle method는 기본적으로 react가 component를 생성하고 없애는 방법이다.

예를 들어서, 앞서 봤던 예시에서 add라는 버튼을 눌러서 component가 업데이트될때 호출되는 다른 function이 있다. 그 전부를 들여다보진 않을거고, 우리가 필요로하는 가장 중요한 것 위주로 살펴보겠다.

### 1. Mounting : 컴포넌트 초기 생성

Mounting은 "태어나는 것"과 같다.
여기에 해당되는 주요한 함수들은 다음과 같다.

- `constructor()` <br>
  컴포넌트 생성자 함수. 컴포넌트가 새로 만들어질때마다 이 함수가 호출되는데,
  이 함수는 javascript class 개념에서 온 함수이다.
  이 부분을 이해하기 위해서는 javascript에서 객체와 class에 대한 공부가 선행되어야 하는데 큰 일은 아니다.

- `render()` <br>
  컴포넌트가 mount될때, 컴포넌트가 스크린에 표시될때, 컴포넌트가 웹사이트에 갈때
  가장 먼저 실행되는 함수가 바로 constructor()이고, 그 다음 render()가 실행된다.

- `componentDidMount()` <br>
  컴포넌트가 render된 후 실행되는 함수.
  "이봐, 이 컴포넌트는 처음 render됐어"라고 알려주는 녀석이다.
  그래서 순서는 constructor() -> render() -> componentDidMount() 순이다.
  주로 해당 컴포넌트에서 필요로하는 데이터를 요청하기 위해 axios, fetch등을 통하여 요청을 하는 작업을 진행할때 사용한다.

### 2. Updating : 컴포넌트 업데이트

Updating은 언제 일어날까? 바로 나자신에 의해서다.
내가 render 함수 안에 정의해놓은 이벤트를 실행할때마다 component의 state가 업데이트되고 이게 바로 component cycle에서 update에 해당한다.

여기에 해당되는 주요한 함수는 다음이다.

- `render()` <br>

* `shouldComponentUpdate()` <br>
  이 api는 컴포넌틀를 최적화하는 작업에서 매우 유용하게 사용된다. 리액트는 변화가 발생하는 부분만 업데이트를 해줘서 효율적이라는 특징이 있는데, 변화가 발생한 부분만 감지해내기 위해서는 Virtual DOM에 한 번 그려줘야한다. 즉, 현재 컴포넌트 상태가 업데이트 되지 않아도 부모 컴포넌트가 리렌더링되면 자식들도 렌더링 된다. 물론 변화가 없으면 DOM은 조작하지 않게 되기 때문에 CPU를 엄청나게 사용하는 건 아니지만 어쨌든 낭비되는 CPU 처리량이 발생할 수 밖에 없다. 이 낭비를 줄이기 위해서 `shouldComponentUpdate`를 사용한다. 기본적으로는 true를 반환하고, 조건에 따라 false를 반환하면 해당 조건에서는 render 함수를 실행하지 않게 할 수 있다.

  <br>

* `componentDidUpdate()` <br>
  componentDidUpdate()는 컴포넌트가 업데이트된 직후에 실행되는 함수이다.
  그래서 setState()를 호출하면 먼저 render()가 실행되고, 업데이트가 완료되면 componentUpdate()가 실행되는 것이다. 이 시점에서는 this.prop과 this.state가 바뀌어있다.

### 3. Unmounting : 컴포넌트 제거

컴포넌트가 더이상 필요하지 않게 되었을때 컴포넌트를 죽게 할 수 있다.
어떻게 컴포넌트가 죽을까?
페이지를 바꿀때, 혹은 state를 이용해서 컴포넌트를 교체할 때 등. 컴포넌트를 제거하는데에는 다양한 방법이 있다.

- `componentWillUnmount()` <br>

  이 함수는 컴포넌트가 죽기 전에 실행되는 함수이다. 시뮬레이션을 통해 볼 수는 없지만 내가 컴포넌트를 생성하고 그 컴포넌트가 죽을때마다 호출할 수 있다. 주로 등록했던 이벤트를 제거하는 용도로 사용한다.

이제 컴포넌트의 라이프사이클을 이용해서 내가 원하는대로 컴포넌트를 조작할 수 있게 되었다. 우왕ㅋ굳ㅋ

<br>
<br>
<br>

_이 글은 velopert님의 [리액트 튜토리얼](https://velopert.com/3631), [Nomad Coders Acacdemy](https://academy.nomadcoders.co/courses/216871/lectures/10881303)의 강의를 참고하여 작성했습니다._
