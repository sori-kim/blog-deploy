---
title: '[React Basis] 안녕, 리액트? + 컴포넌트를 만드는 첫번째 방법: Function Component'
date: 2020-05-04 00:21:13
category: 'category1'
draft: false
---

## 리액트(react.js)를 알아보자구~

![react_graph](https://user-images.githubusercontent.com/60246689/81028739-a667b900-8ebd-11ea-8a45-8e767be62efc.png)

리액트는 페이스북과 인스타그램에서 사용자 경험을 향상하기 위해 만든 라이브러리로, 브라우저가 동적으로 기능할 때 서버에서 코드를 받아 다시 렌더링해야되는 문제(server side rendering)를 해결하기 위해 만들어졌다.
방대한 자료와 유연함이라는 장점 때문에 최근 몇년 간 가장 인기 있는 프론트엔드 프레임워크 자리를 지키고 있다.

## 리액트의 특징

### 컴포넌트 기반의 화면 구성

리액트는 화면의 한 부분을 `컴포넌트` 라는 단위로 나누어서 독립적으로 관리할 수 있게 한다. 규모가 큰 프로젝트에서 코드를 기능별로 컴포넌트화하면 코드를 관리하기 쉽고 반복되는 부분을 대체할 수 있게 해주어서 `코드 재사용성`을 높여준다. 또 컴포넌트 기반의 화면을 구성을 하면 빠르고 효율적으로 화면을 구성할 수 있다.

### JSX: 템플릿을 사용하지 않는다.

JSX는 리액트에서 커스텀한 자바스크립트 확장 문법인데, 자바스크립트에서 html과 자바스크립트 변수, 속성 값을 사용하게 해준다.
별도의 템플릿은 사용하지 않는다.

### Virtual DOM: 전체 DOM을 다시 그리지 않는다.

`Virtual DOM`은 가상의 DOM이다. 어떤 변화가 일어나면, 실제로 브라우저의 DOM 에 새로운걸 넣는것이 아니라, 자바스크립트로 이뤄진 가상 DOM 에 한번 렌더링을 하고, 기존의 DOM 과 비교를 한 다음에 정말 변화가 필요한 곳에만 업데이트를 해주는 것이다.

기존의 자바스크립트 DOM을 이용한 방식은 매번 DOM전체를 직접 접근하여 변화가 있을때마다 html/css/js파일 전체를 다시 리랜더링 하기 때문에 느려질 수 밖에 없었다. 하지만 리액트는 가상DOM을 이용해서 더욱 효과적으로 화면 렌더링을 할 수 있게 만들었다.

## 그래서, 리액트 어떻게 쓰냐구?

### React Components

컴포넌트는 기본적으로 html을 반환(return)하는 함수인데, 리액트는 기본적으로 컴포넌트와 함께 동작한다. 모든것은 컴포넌트.
이제부터 리액트를 쓰는 모든 과정은 컴포넌트를 만들고, 컴포넌트가 예뻐보이게 만들거고, 컴포넌트가 데이터를 보여주게 하는 과정이 될 거다.
`이제 컴포넌트는 너가 새롭게 제일 좋아하는 단어가 될 것~`

### How do we make components ?

1 .js파일 하나를 생성한다.
그리고 반드시 react를 임포트 해줘야한다. 그렇지 않으면 리액트는 이 파일에서 jsx가 있는 컴포넌트를 사용한다는걸 인식하지 못한다.

```javascript
import React from 'react'
```

2. 대문자로 시작하는 이름의 함수를 만들고 실행내용으로 원하는 html 태그를 입력한다.

```javascript
function Potato() {
  return <h1> I love potato. </h1>
}
```

3. export한다.
   이렇게만 하고 마치면 vscode에서 해당값이 선언되었지만 아직 읽히지 않았다고 알려준다.
   아직 export를 하지 않았기 때문!

```javascript
export default Potato
```

### How do we use components?

컴포넌트를 만들고 실행하고 싶을때는 다음과 같은 처음 보는 모양새를 갖는다.
`<Potato />`
리액트 고유의 문법이라서 꼭 이렇게 지켜줘야지 html이 정해진 함수 내용으로 반환된다.
javascript안에 html 구조가 있는 이러한 조합의 구조를 `jsx`라고 부른다.
이건 리액트에서 나온 매우 커스텀한 유일한 개념이고 나머지는 javascript에서 배우는 문법을 그대로 가져간다. (variables, array, classes, map...)

자 이제 함수를 사용하면 되는데, 어떻게 사용할 수 있을까?

#### 핵심: 하나의 앱에서 하나의 컴포넌트만 렌더링 가능하다.

반드시 알아둬야할 건 리액트는 `하나의 앱에서 단 하나의 컴포넌트만 렌더링 할 수 있다`는 것이다.
그래서 이미 렌더링한 컴포넌트가 있다면, 그 컴포넌트 안에 내가 새로만든 컴포넌트를 임포트에서 사용해야한다.

app.js 파일안에 있는 `<App />` 컴포넌트안에 `<Potato />`를 임포트 해보자.

```javascript
import Potato from './Potato'

function Potato() {
  return 'I love potato'
}

function App() {
  return (
    <div>
      <h1> Hello </h1>
      <Potato />
    </div>
  )
}

export default App
```

참고로 여기서 `. /`은 같은 디렉토리라는 뜻이다. `Potato.js`와 `App.js`가 같은 디렉토리 안에 있기 때문에 임포트할때 저렇게 적어주는 것이다.

그리고 내가 원하는 위치에서 `Potato` 컴포넌트를 사용해주면 끝이다.
다음 포스팅에서는 본격적으로 컴포넌트에 데이터를 전달하는 방법에 대해 알아보겠다.

<br>
<br>
<br>

_[개발자스럽다](https://blog.gaerae.com/2016/04/hello-react.html)_
<br>
_[NomadCoderAcademy](https://academy.nomadcoders.co/courses/enrolled/216871)_
