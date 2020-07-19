---
title: " [인스타클론] ⭐️ fetch() GET, POST  (회원가입,로그인,피드 댓글 불러오기 기능) "
date: "2020-05-24T02:12:03.284Z"
template: "post"
draft: false
slug: "react/200524"
category: "react"
tags:
  - "react"
  - "westagram"
description: "인스타그램 클론한 앱에 드디어 백엔드와 연결해보는 미니 프로젝트를 진행했다. 
크게 회원가입, 로그인, 메인의 피드중에서 댓글을 불러오는 기능을 구현했고 코드를 정리해두려고 한다."
---

## 인스타그램 미니 프로젝트 🧩

지난번에 배운 리액트 jsx 문법, CRA 초기세팅, props & states, routing에 이어서 이번에는 드디어 백엔드와 연결해보는 미니 프로젝트를 진행했다. <br>
크게 회원가입, 로그인, 메인의 피드중에서 댓글을 불러오는 기능을 구현해봤고 그 코드를 정리해두려고 한다.

# fetch

Fetch는 이용하면 서버와의 데이터 통신을 할 수 있게 하는 비동기 함수 API이다.
<br>

[참고] **비동기(Asynchronous)** <br>
자바스크립트는 기본적으로 위에서 아래로 코드를 순서대로 실행하는데, 하나의 코드가 끝난 후에 다음 코드로 넘어간다.
그런데 fetch는 비동기요청이기 때문에 함수가 끝날때까지 기다리는게 아니라 호출되어서 요청을 보내고 다음 줄로 넘어간다.
대부분의 함수는 동기 함수인데 **setState**나 **fetch** 같은 특별한 함수만 비동기함수이다.

## 1. fetch() GET

fetch를 사용할때 특정한 요청 방식을 설정하지 않으면 기본 GET이다.
GET fetch는 주로 api주소에 연결하여 데이터를 로딩하는 용도로 사용되고, 그렇기 때문에 컴포넌트 라이프사이클의 `componentDidMount()` 안에서 실행되어야한다.

참고로 컴포넌트가 마운트 되었다는 뜻은 화면에 보여진다는 뜻인데 (jsx를 리턴하는 함수인 render가 실행되어 화면이 렌더링 되면 이걸 마운트라고 한다.) 최초의 화면이 띄워진 후 곧바로 데이터 로딩이 이루어질 수 있도록 `componentDidMount()` 안에서 GET요청으로 fetch를 실행한다.

fetch는 앞서 말했듯이 비동기 함수이기 때문에 이 요청이 완료되었을때 실행할 다음 단계를 설정할 수 있는 함수도 있다. 바로 `then()`. 백엔드에서 보내주는 데이터는 보통 JSON이라는 데이터 형식으로 오는데 이 JSON타입은 자바스크립트에서 이해를 못하기 때문에 최초의 응답결과를 `response.json()`을 먼저 실행해서 자바스크립트화 해줘야지 사용할 수 있다. <br>
그리고 그 후에는 데이터가 잘 받아졌는지 콘솔을 한번 찍어본 후, 내가 원하는 결과대로 데이터가 잘 받아졌다면 이 데이터를 `setState()`를 이용해서 상태값으로 적용해주면 드디어 서버에서 가져온 데이터를 리액트에서 접근할 수 있게 된다.

<br>

```jsx
componentDidMount(
  fetch(api주소)
    .then((response) => response.json()) //자바스크립트화 된 데이터
    .then((response) => this.setState({ data: response }))
);
```

<br>

[참고] <br>
setState도 비동기기 때문에 이것도 하나의 요청이다. 이 요청이 response가 오면 상태값이 바뀌었기 때문에 render함수가 실행된다.
그렇기 때문에 state가 바뀐 결과를 보고싶다면 두가지 방법이 있는데, <br>
첫번째는 render 안에서 콘솔을 찍는 것이다. 이 플로우 때문에 콘솔에는 마운트 됐을때와 상태값이 바뀌었을때 총 두번의 state값이 찍힌다.
setState가 있는 함수 안에서 콘솔에 찍으면 비동기 특성때문에 값이 하나씩 밀린다는 점을 염두에 두어야 한다. <br>
그리고 두번째 방법은 setState의 두번째 인자로 콘솔로그 함수를 콜백함수로 넣으면 된다.

<br>

## 2. fetch 회원가입 POST

회원가입 같은 기능을 만들기 위해서는 POST 요청을 사용해야하는데, `fetch(url, {요청정보})` 형식을 사용한다.
그리고 포스트 요청은 componentDidMount 안에 쓰는 게 아니라, 회원가입은 유저가 정보입력을 한 뒤 회원가입 버튼을 눌렀을때 정보가 백엔드로 갈 수 있도록 onClick 이벤트핸들러 같은 트리거에 fetch함수를 넣어줘야한다.

POST 요청은 기본적으로 보내는 것에 의의를 두는 것이기 때문에 답변이 돌아오는것을 기대하면 안된다. 특히 회원가입은 단순 POST요청이기 때문에 정보만 보내고 끝이다.
(status code는 들어오는게 맞지만 데이터가 들어오지 않는다는 의미이다.)

[회원가입할때 백앤드에 요구해야하는것]

1. api주소(+엔드포인트)
2. 바디에 담아 보낼 내용의 키값들, 꼭 들어가야하는 데이터와 그러지 않아도 되는 데이터

```jsx
class SignUp_sr extends React.Component {
  constructor() {
    super();
    this.state = {
      email_or_phone: "",
      name: "",
      id: "",
      pw: "",
    };
  }

  //회원가입 버튼 클릭 시, input에 입력된 모든 정보가 POST요청으로 서버에 보내진다.
  handleOnClick = () => {
    fetch("http://10.58.3.147:8000/account/sign-up", {
      method: "POST",
      headers: {
        "content-type": "aplication/json", //담아보내는 데이터가 json형식이라는 뜻. 안써줘도 무방함
      },
      body: JSON.stringify({
        email_or_phone: this.state.email_or_phone,
        realname: this.state.name,
        username: this.state.id,
        password: this.state.pw,
        //포스트 메서드의 바디 안에 있는 키는 내가 정하는것이 아닌, 백엔드에서 요구하는 키로 적어야한다.
      }),
    })
      .then((response) => response.json())
      .then(alert("회원가입에 성공했습니다. 👏🏻"));
  };
```

[참고] 회원가입할때 만약 중복된 데이터가 있거나 적절하지 않은 형식일때는, 조건을 걸러서 알림창을 띄울 수 있다.
원칙적으로는 response에 오는 데이터 중 하나인 status code를 이용하여 조건문을 만들어야하는데 status code는 `response.json()`이라는 자바스크립트화를 하기 전에 부를 수 있다는걸 알아두자. 이번에는 단순하게 백엔드에서 설정한 response message를 이용해서 조건문을 만들었다.

```jsx
.then(response => {
    if(response.message === 'INVAILD EMAIL'){
        alert('이메일을 확인해주세요!')
    }
})
```

<br>

## 3. 로그인 POST

회원가입과 거의 똑같은데 response를 어떻게 처리하느냐만 다르다. <br>
로그인은 유저가 아이디와 비밀번호를 입력했을때 백엔드에 이 데이터를 보내서 해당 정보가 회원정보 DB에 있는 데이터와 일치하는지 비교한 뒤,
일치하면 access token 이라고 하는 고유한 키를 유저마다 부여하여 response에 담아 줄 것이다. <br>
그러면 프론트엔드는 이 토큰키를 브라우저의 로컬스토리지에 저장해두어 유저가 식별될 수 있도록 해야 한다.

```jsx
export class Login_sr extends React.Component {
  constructor() {
    super();
    this.state = {
      id: "",
      email_or_phone: "",
      pw: "",
      isActive: false,
    };
  }

  //로그인 버튼 클릭 시, 입력된 id,pw 데이터를 POST요청으로 보내고 토큰을 받아서 LS에 저장하고 메인으로 이동
  handleOnClick = () => {
    fetch("http://10.58.0.163:8000/account/sign-in", {
      method: "POST",
      headers: {
        "content-type": "aplication/json",
      },
      body: JSON.stringify({
        username: this.state.id,
        email_or_phone: this.state.email_or_phone,
        password: this.state.pw,
      }),
    })
      .then((response) => response.json())
      .then((response) => {
        if (response.message === "login successful!") {
          localStorage.setItem("access_token", response.token);
          this.goToMain.call(this); //메인으로 가는 라우트 기능
        } else {
          alert("입력한 정보가 회원정보와 일치하지 않습니다.");
        }
      });
  };
```

<br>

## 4. 피드의 댓글 불러오기 (GET & POST)

댓글 기능은 요청이 두개 들어가야한다.

1. get요청: componentDidMount에 fetch로 토큰을 담아서 보내면 ('이 유저에 대한 정보를 다 가져와줘') 백엔드에서 코멘트리스트를 전체 다 보내줄것이다. 그러면 그 돌아오는 response를 setState한 뒤 이 데이터를 이용해서 댓글이 화면에 보여질 수 있게 map함수를 돌린다. 데이터 로딩이 마쳐진 후 댓글이 화면에 렌더된다. <br>

2. post요청: 댓글의 입력 버튼을 실행하면 post요청이 실행되어서 새로운 댓글이 추가된다. <br>
   이때 중요한 점은 각 유저가 식별되어야 하기 때문에 로그인 할 때 로컬스토리지에 저장해두었던 토큰을 불러와서 헤더에 담아서 보내야한다. <br>

   하지만 아쉽게도 이번에 댓글 입력 버튼 클릭했을때 댓글이 실시간으로 어펜딩이 되는 기능은 구현하지 못했다. 이 기능을 구현하기 위해서는 원래 POST 요청으로 입력된 데이터를 담아보내고,
   다시 GET 요청으로 코멘트 리스트를 받아와야하는데, fetch가 비동기 특성이 있어서 이 기능이 순서대로 이루어지게 하기 위해서는 setTimeout 같은 시간 조절 기능까지 이용해서 적절한 시간 순서로 보내지고 받아오는 순서가 이루어지게 해야한다. 근데 이 단계까지 가기에 아직 실력이 부족해서 이번에는 GET과 POST를 한번씩 사용해보는 정도로 만족하기로 했다.

### 4-1. GET요청: 댓글 리스트 불러오기 (조회)

```jsx
export class Feed_sr extends React.Component {
  constructor() {
    super();
    this.state = {
      comments: [],
      text: "",
      username: "",
      isActive: false,
    };
  }

  componentDidMount() {
    const token = localStorage.getItem("access_token");
    fetch("http://10.58.0.163:8000/feed/comment", {
      method: "GET",
      headers: {
        "Content-type": "aplicaiton/json",
        Authorization: token, //어떤 유저인지 백엔드가 식별할 수 있도록 헤더에 토큰을 담아보낸다.
      },
    })
      .then((response) => response.json())
      .then((response) => {
        let arr = [];
        response.comments.map((txt) => arr.push(txt));
        this.setState({
          comments: arr,
        });
        console.log(this.state);
      });
  }
```

<br>

이번 프로젝트에서 제일 어렵고 오래 걸렸던 부분이었다. 단순히 response를 받아와서 통째로 setState를 하면 된다고 생각했지만, 계속 map 함수 오류가 나서 map함수를 잘못 사용하고 있다고 생각했는데 해결을 못하고 있었는데, 알고보니 백엔드에서 보내주는 데이터의 모양이 내가 생각한 것과 달랐던 거다. 결국 해결한 방식은 다음과 같다. <br>

POST 요청에 대한 응답이 왔을때,

1. arr라는 빈 배열을 만든다. <br>
   콘솔에 찍어보니 response는 아래와 같은 형태로 왔다.

```jsx
comments = [
        { "username: "",
        "content" : ""
        },
 {}, {}, {}, ..., {}
]
```

<br>

2. `response.comments.map()` 을 이용해서 배열의 요소 하나씩 접근해서 각 요소를 빈 배열에 푸쉬했다.

3. 그리고 이 새로만든 배열을 comments의 상태값으로 새로 설정했다.

```jsx
this.setState({
  comments: arr,
});
```

<br>

### 4-2. POST요청: 댓글 추가하기

클릭이 일어났을때 새로 입력한 댓글이 백엔드로 보내지도록 POST요청을 했다.

```jsx
//댓글창에 입력 시 state 변경
handleValue = (event) => {
  this.setState({
    text: event.target.value,
    isActive: true,
  });
};

handleOnClick = () => {
  const comments = this.state.comments;
  const new_arr = comments.concat(this.state.text);
  const token = localStorage.getItem("access_token");

  this.setState({
    comments: new_arr,
    text: "",
  });

  fetch("http://10.58.0.163:8000/feed/comment", {
    method: "POST",
    headers: {
      "content-type": "aplicaiton/json",
      Authorization: token,
    },
    body: JSON.stringify({
      text: this.state.text,
      feed_id: 1,
    }),
  })
    .then((response) => response.json())
    .then((response) => this.setState({ comments: response }));
};
```

<br>

댓글리스트가 화면에 보여지는건 다음과 같이 구성했다.

```jsx
  render() {
    const reply = this.state.comments;

    const commentList = reply.map((element, index) => (
      <li key={index} className="new">
        {element.username} {element.content}
      </li>
    ));

    return (
    <ul className="comments-list">{commentList}</ul>
    )
  }
```

이렇게 댓글 기능 구현까지 완료했다!

처음으로 백엔드와 연동해서 동적인 데이터를 핸들링해보니 확실히 쉽지 않았다... 😂😂😂
핵심은 이 두가지인 것 같다.

- fetch를 이용해서 받아온 데이터의 형태를 잘 확인한 뒤 setState를 잘 하기.
- map을 잘 돌려서 화면에 뿌려주기

그래서 프로젝트 시작하기 전, 그리고 플젝 하면서도 이 루틴에 익숙해지는 게 목표이다. <br>
화이팅 화이팅,,!

<br>
<br>
