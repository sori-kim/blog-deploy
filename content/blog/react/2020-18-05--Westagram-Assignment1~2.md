---
title: " [인스타클론] React Props & States 활용, 이벤트핸들링 "
date: "2020-05-18T01:12:03.284Z"
template: "post"
draft: false
slug: "react/200518"
category: "react"
tags:
  - "react"
  - "westagram"
description: "지금 진행하고 있는 인스타그램 클론 미니프로젝트에서 states를 이용해서 데이터를 다루는 연습과 이벤트 핸들링을 연습해봤다. "
---

<br>

지금 진행하고 있는 인스타그램 클론 미니프로젝트를 개략적으로 말하자면, 먼저 첫번째 스텝으로 바닐라자바스크립트로 만들었던 모든 코드를 리액트 jsx 문법과 sass로 바꿨다. 그리고 두번째 단계로 react router를 이용해서 라우팅 기능을 넣어서 로그인 버튼을 클릭했을때 메인 페이지가 나오도록 했다. <br>

이제 세번째 스텝 states를 이용해서 데이터를 다루는 연습과 이벤트 핸들링을 연습해봤다.
이 포스팅은 그 과정에 대한 간단한 정리이다.

## ✔️Assignment

1. 로그인 페이지 - 로그인 버튼 눌렀을 때, 사용자가 입력한 id와 pw 콘솔에 나올 수 있게 하기
2. 로그인 페이지 - 사용자가 입력한 id와 pw가 기준(id - '@' 포함 / pw - 5글자 이상)에 맞는 경우에만 로그인 버튼 색상이 활성화될 수 있도록 하기

## 1. 로그인 버튼 눌렀을때 입력된 값 콘솔에 나오게 하기

다음과 같은 로직으로 생각을 해봤다.

- Login 클래스형 컴포넌트 생성 / state 기본값 설정
- onKeyUp 이벤트를 이용하여 input에 값이 입력되었을때 setState()로 상태를 변경한다.
- onClick 이벤트를 이용하여 버튼이 클릭되었을때 state값이 콘솔에 찍히게 한다.

<br>

```jsx
import React from "react";
import { Link, withRouter } from "react-router-dom";
import "../../pages/Login/Login.scss";

export class Login extends React.Component {
  constructor() {
    super();
    this.state = {
      id: "",
      pw: "",
    };
  }

  handleOnClick = () => {
    this.clickValue();
    this.goToMain.call(this); //라우팅 함수
  };

  clickValue = (event) => {
    console.log(this.state.id, this.state.pw);
  };

  handlePw = (event) => {
    this.setState({ pw: event.target.value });
  };

  handleId = (event) => {
    this.setState({ id: event.target.value });
  };

  render() {
    return (
      <main className="Login">
        <div className="login-wrapper">
          <img className="logo" alt="logo" src={logoImg} />
          <input
            className="id-input login"
            type="text"
            placeholder="전화번호,사용자 이름 또는 이메일"
            onChange={this.handleId}
          />
          <input
            className="password-input login"
            type="password"
            placeholder="비밀번호"
            onChange={this.handlePw}
          />
          <button
            onClick={this.handleOnClick}
          >로그인</button>
    );
  }
}

export default withRouter(Login);
```

<br>

<img width="479" alt="스크린샷 2020-05-18 오전 11 02 02" src="https://user-images.githubusercontent.com/60246689/82167540-25fe7a80-98f7-11ea-918d-afc9c5d28e77.png">

<img width="401" alt="스크린샷 2020-05-18 오전 11 02 21" src="https://user-images.githubusercontent.com/60246689/82167571-37478700-98f7-11ea-9aca-d7ae1d0ff688.png">

이 결과 입력된 값이 콘솔에 잘 찍혔다. 후후후

<br>
<br>

## 2.입력한 id와 pw가 기준(id - @포함 / pw - 5글자 이상)에 맞는 경우에만 로그인 버튼 색상이 활성화 될 수 있도록 하기

이 경우에, 입력된 Id와 pw에 대해 조건문을 걸어서 기능을 만들었다.

```jsx
activeBtn = () => {
  this.state.id.includes("@") && this.state.pw.length >= 5
    ? this.setState({ isActive: true })
    : this.setState({ isActive: false });
};
```

그리고 두 input을 감싸고 있는 wrapper 태그에 이벤트 함수를 걸었다.

```jsx
 <div className="login-wrapper" onKeyUp={this.activeBtn}>
```

다음으로 미리 만들어놓은 버튼 활성화 css 태그를 삼항연산자로 적용했다.

```css
.activeBtn {
  opacity: 1;
  cursor: pointer;
}
```

```jsx
<button
  onClick={this.handleOnClick}
  className={this.state.isActive ? "login-btn activeBtn" : "login-btn"}
>
  로그인
</button>
```

<img width="406" alt="스크린샷 2020-05-18 오전 11 25 15" src="https://user-images.githubusercontent.com/60246689/82168676-47ad3100-98fa-11ea-8b69-d8f015536de9.png">

이렇게해서 아이디에 @가 있을때, 그리고 패스워드가 5글자 이상일때 버튼이 활성화 되었다.

<br>
<br>
