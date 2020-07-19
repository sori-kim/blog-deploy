---
title: '[React Basis] Session #2  React router'
date: '2020-05-16T02:12:03.284Z'
template: 'post'
draft: false
slug: 'react/200512/1'
category: 'react'
tags:
  - 'react'

description: 'react에서 라우팅을 하는 두가지 방법 Link 컴포넌트와 withReact HOC에 대해 정리한다.'
---

## 1. React Router

react는 다른 프레임워크들과 달리 라우팅을 위한 로직이 들어있지 않아서
`React Router` 라는 기능을 추가해서 구현해야한다. <br>
내가 지금 만들고 있는 instagram 클론 앱에 라우팅 기능을 넣어보는 법을 정리해두려고 한다.

### react-router 설치

```
npm install react-router-dom --save
```

### react-router 사용

public 폴더에 `Routes.js`라는 컴포넌트 파일을 생성한다.
이제 이 파일에 내가 작업한 모든 리액트 컴포넌트들을 한 데 모아서 라우팅 작업을 진행하게 될 것이다.

이 Routes.js 컴포넌트 내부 코드를 작성하기 전에 먼저 해야할 일이 있다.

CRA로 만든 앱에 route를 추가해서 사용하려면 최종적으로 렌더링되는 컴포넌트를 라우팅을 설정한 컴포넌트로 대치해야한다.
index.js 파일을 통해 기존에 만들었던 컴포넌트들을 render 하고 있었는데, 이제는 Routes 컴포넌트가 최상단 컴포넌트로 위치하게 될 것이다.

```jsx
import Routes from './Routes'

ReactDOM.render(<Routes />, document.getElementById('root'))
```

### Routes 컴포넌트 구현하기

```jsx
import React from 'react'
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom'
import Login from './pages/Login/Login'
import Main from './pages/Main/Main'

class Routes extends React.Component {
  render() {
    return (
      <Router>
        <Switch>
          <Route exact path="/" component={Login} />
          <Route exact path="/main" component={Main} />
        </Switch>
      </Router>
    )
  }
}

export default Routes
```

<br>

### Route 이동하기

Route 이동하는 방법은 기본적으로 두 가지가 있는데, 바로 `Link` 컴포넌트를 사용한는 방법과 `withRouter HOC`를 이용하는 방법이다.

#### 1) Link 컴포넌트를 사용하는 방법

Link 컴포넌트는 react-router-dom에서 제공하는 컴포넌트로, DOM에서 `<a>`로 변환이 된다. <br>
그래서 Link 컴포넌트는 남발해서 사용하는 게 아니라 상황에 맞게 사용을 해야한다. <br>
만약 페이지가 클릭 시 항상 이동해야하는 경우가 아닌, 로그인 기능 등에서는 조건에 따라 페이지가 이동되어야 하기 때문에 이 Link 컴포넌트를 사용하면 안된다.

이럴때는 바로 아래에서 설명할 withRouter HOC를 이용해서 구현하면 된다.

#### 2) withRouter HOC로 구현하는 방법

이 방법은 Link를 사용하는게 아니라, 요소에 onClick 이벤트를 달아서 이동하고 싶은 곳으로 넘기는 방법이다. <br>
'로그인'이라는 버튼을 클릭했을때 onClick 이벤트가 실행되도록 `goToMain()`이라는 함수를 값으로 줬다.
그리고 이 함수의 실행 내용을 보면, <br>
this.props의 history에 접근해서 이동하는 방식을 취하고 있고
push 메서드에 이동할 path를 인자로 넘겨줘서 해당 라우트로 이동할 수 있게 된다.

```jsx
import React from "react";
import { Link, withRouter } from "react-router-dom";


export class Login extends React.Component {

  goToMain() {
    this.props.history.push("/main");
  }

  render() {
    return (
      <main className="Login">
        <button onClick={this.goToMain.bind(this)}>
          로그인
        </button>
      </main>
    )
  }

  export default withRouter(Login);
```

이 컴포넌트에서 props에 route 정보(history)를 받으려면 export하는 class에 withRouter로 줘야한다. 이렇게 withRouter같이 해당 컴포넌트를 감싸주는 것을 `higher-order component(이하 HOC)` 라고 한다.

HOC는 react에서 컴포넌트의 공통부분을 구현하는 패턴을 만드는 일종의 고급 기능인데, 컴포넌트를 인자로 받고 리턴하는 함수이다. 이 부분은 아직 배우기 어려운 내용이라 이번에는 react-router에서 제공하는 withRouter라는 HOC를 사용하고, 우리는 props에서 라우팅 정보만 편하게 받는 식으로 사용하고 나중에 심화적인 부분을 공부해야겠다.
그리고 아직 history나 push, bind 메소드같은게 생소하지만 대략적으로 이런 흐름을 통해 라우팅이 이루어질 수 있다는것만 기억하자.

<br>
<br>
<br>
<br>
<br>

_이글은 wecode bootcamp의 멘토 [예리님 블로그](https://yeri-kim.github.io/posts/react-jsx/) 내용과 리액트 세션을 학습하고 정리한 포스팅입니다._
