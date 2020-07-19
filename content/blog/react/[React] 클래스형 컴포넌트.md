---
title: '[React Basis] 컴포넌트를 만드는 두번째 방법: Class Component'
date: 2020-05-07 02:21:13
category: 'react'
draft: false
---

리액트에서 컴포넌트를 만드는 방법은 크게 두가지로 나뉜다.
바로 `함수형 컴포넌트`와 `클래스형 컴포넌트`를 만드는 것인데
각각의 방법은 다루게 되는 데이터의 성질도 다르다.

함수형 컴포넌트는 `props`를 데이터로 다루고, 클래스형 컴포넌트는 `state`를 데이터로 다룬다.
각각에 대해 짧게 설명하자면 `props`는 부모 컴포넌트가 자식 컴포넌트에게 주는 값이다. 자식 컴포넌트에서는 props를 받아오기만 하고, 받아온 props를 직접 수정할 수는 없다. 단순히 데이터를 받아서 사용만 하는 목적이라면 함수형 컴포넌트를 만들어서 props를 데이터로 사용하면 된다.

반면에 `state`는 컴포넌트 내부에서 선언하고 내부에서 동적으로 값을 변경할 수 있다. 뒤에서 배우게 될 Life Cycle API를 사용할 수 있다.

사실 함수형 컴포넌트와 props는 처음에 리액트에서 컴포넌트를 만드는법을 다룰때 봤던 내용이라서 이번 포스팅에서는 클래스형 컴포넌트를 만드는 방법과 이 형태의 컴포넌트에서 데이터, 즉 state를 다루는법에 대해 주로 정리를 해보려고 한다.

## 함수형 컴포넌트 & 클래스형 컴포넌트?

지금까지 우리가 배웠던 아래 같이 생긴 컴포넌트는 함수형 컴포넌트(function component)다.

```javascript
function App() {
  return (
    <div>
      {foodILike.map(dish => (
        <Food name={dish.name} picture={dishi.image} />
      ))}
    </div>
  )
}
```

그렇다면 class component는 어떻게 생겼을까?

```javascript
import React from 'react'

class App extends React.Component {
  render() {}
}
```

이 단계가 바로 class component를 만들기 위해 필수적이다.
리액트 컴포넌트는 방대한 기능들을 담고 있는데 컴포넌트를 만들때마다 모든 기능을 다 가져와서 사용할 필요가 없기 때문에 `extends` 표현을 사용해서 필요로 하는 기능만 가져와서 사용하려고 하는 것이다. 그 중 하나가 지금부터 이야기할 state다.

혹은 아래와 같은 방식으로도 extend 할 수도 있다.

```javascript
import React, { Component } from 'react'
class App extends Component {
  render() {}
}
```

<br>

클래스 컴포넌트와 리액트 컴포넌트에 대한 설명을 이해하기 쉽게 비유를 들어보자. <br>
baby는 human에서 확장(extend)되어 human의 특징을 그대로 가지게 된다.
samsung phone은 cell phone에서 확장되어 cell phone의 attributes들, 이를테면 screen, camera 등을 가지게 된다. 그리고 samsung phone은 iphone과 이 attributes들을 공유한다.

이렇듯 App 클래스는 react 클래스 컴포넌트로부터 속성값들을 확장해서 가져오고 있다.
이때 주의할 점은 class component는 함수가 아니기 때문에 값을 return 하는 특징을 가지고 있는게 아니라 `render`를 한다. 그리고 이 render method는 당연하게도 react component가 가지고 있기 때문에 여기서 확장된 App class component 역시 이 method를 사용할 수 있는것이다.

정리해보면, `function component`는 `function`이고 뭔가를 `return` 하여 화면에 표시해준다. 하지만 `class component`는 react component라는 가장 상위의 컴포넌트로 부터 `확장(extend)`된 컴포넌트인데 react가 실행되면 자동적으로 모든 class component의 `render method`가 실행된다.

## State: 동적 데이터가 담기는 공간(객체)

그렇다면 왜 굳이 function component를 냅두고 class component라는 어려운 녀석을 이야기해야하는걸까? <br>
그 이유는 class component가 `state`라고 부르는 우리가 필요로 하는 개념을 갖고 있기 때문이다.

state는 component의 데이터를 넣을 공간인 object이다.
그런데 여기서 말하는 데이터는 앞서 이야기했던 dynamic data, 즉 서버에서 받아오는 역동적인 데이터다.
그래서 state 안에 바뀌게 될 데이터를 넣고 이용하면 된다.
그리고 이 녀석은 객체이기 때문에 class component에서 사용하기 위해서는
`{this.state.count}` 이런 식으로 중괄호 안에 this 키워드를 사용해야한다.

```javascript
class App extends React.Component {
  state = {
    count: 0,
  }

  render() {
    return <h1> The number is:{this.state.count}</h1>
  }
}
```

이렇게 입력하고 나면 화면에 state가 반영된 값이 화면에 출력된다.
![](https://images.velog.io/images/rimu/post/4a2baa0b-5b69-4517-b33f-27590ad2e5bf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-05-06%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.56.18.png)
<br>
<br>

## State를 이용해서 이벤트 걸기

그런데 여기서 문제는 App이라는 class component에서 데이터를 어떻게 바꿀 것인가이다.
state의 데이터를 역동적으로 만드는걸 배우기 위해, 먼저 클래스컴포넌트에서 이벤트를 실행하는것 부터 알아보자.

```javascript
class App extends React.Component {
  state = {
    count: 0,
  }

  add = () => {
    console.log('add')
  }
  minus = () => {
    console.log('minus')
  }

  render() {
    return (
      <div>
        <h1>The number is: {this.state.count}</h1>
        <button onClick={this.add}>Add</button>
        <button onClick={this.minus}>Minus</button>
      </div>
    )
  }
}
```

<br>

자바스크립트였다면 `addEventlistener`를 이용해서 요소에 이벤트를 걸어줬겠지만 리액트에서는 좀 더 쉽게 이벤트를 걸 수 있다. `onClick`이벤트가 리액트에 기본적으로 내장되어 있어서,
`<button onClick={this.add}>add</button>`에 직접적으로 이벤트를 실행할 수 있다.
위의 코드를 실행하고 콘솔창을 보니 이벤트가 잘 걸린걸 알 수 있다.
<br>

![](https://images.velog.io/images/rimu/post/d056c9ef-bd37-4cfe-9235-f462cfc7ef1e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-05-06%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.12.31.png)

<br>

## state의 데이터를 동적으로 만들기 : setState()

state를 사용할 때 주의할 점이 있다. 바로 state의 상태를 변경하고 싶을때 (여기 담긴 데이터의 값을 변경하고 싶을때) state를 직접적으로 변경하면 안된다는 것이다.
이렇게 시도하면 "Do not mutate state directly. Use setState()" 라는 무서운 경고를 받는다.

그럼 어떻게 하냐고?
render function에서 바꿔줘야하는데, 이때 `setState()`라는 함수를 이용한다.
setState는 새로운 state를 취하게 해주는데 state는 객체이기 때문에 당연히 `{ }`안에 넣어줘야한다. 이렇게 입력하면 add라는 버튼을 누를때마다 count의 값이 1이 된다.

```javascript
add = () => {
  this.setState({ count: 1 })
}
```

이번에는 add를 클릭했을때 1 증가, minus를 클릭했을때 1이 감소하게 만들어보자.

```javascript
add = () => {
  this.setState({ count: this.state.count + 1 })
}
minus = () => {
  this.setState({ count: this.state.count - 1 })
}
```

이게 가능한 원리는 리액트가 너무 똑똑해서 setState 함수를 호출하면 state를 업데이트하고 바로 render function을 호출한다. 그러면서 변화가 있는 부분만 변형해서 보여주는것이다.
이 원리는 앞서 말했던 virtual DOM을 이용하고 있는 건데, 리액트는 깜박이지도 않고 데이터를 업데이트해서 보여준다.

하지만 참고로 이렇게 state에 의존하는 코드는 몇가지 성능문제를 갖고 있기 때문에 사실상 좋은 코드가 아니다. 그래서 this.state가 아니라 current라는 매개변수를 쓰는게 훨씬 더 효과적이다.

```javascript
add = () => {
  this.setState(current => ({ count: current.count + 1 }))
}
```

정리하자면, setState를 호출할때마다 react는 state를 업데이트하고 그 변경된 사항을 반영해서 화면을 render한다! 엄청 중요하니 얼굴에 타투를 하자 ⭐️
(by 니꼬 😂)
