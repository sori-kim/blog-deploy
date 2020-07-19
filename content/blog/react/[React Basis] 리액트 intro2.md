---
title: '[React Basis] 컴포넌트에 정보를 전달하기 1 '
date: 2020-05-05 00:21:13
category: 'react'
draft: false
---

`자 이제 시작이야~ 본격적인 리액트의 세계 속으로!`  
지난 포스팅에서 리액트의 특징 몇가지와 컴포넌트를 만드는 법에 대해 알아봤다. 이번에는 단순히 같은 내용의 컴포넌트를 찍어내는게 아니라,
내가 원하는 데이터에 따라 언제든지 다른 내용을 찍어내는 컴포넌트를 만들어보려고 한다.

## 컴포넌트 컴포넌트, 데이터 전달하기

<strong>핵심: JSX에서는 컴포넌트에서 컴포넌트로 정보를 보낼 수 있다. </strong> <br>
리액트가 멋진 이유는 이렇게 재사용 가능한 컴포넌트를 만들 수 있다는 점이다.
그럼 이제 본격적으로 컴포넌트에서 어떻게 데이터를 전달하는지 알아보겠다.

### Basis

컴포넌트에 정보를 보낼때는 html에 속성값을 주는것과 굉장히 비슷한 모양을 하고 있다. `<Food name="kimchi" />` <br>
이 방식을 구체적으로 말하면 ‘Food component에 kimchi라는 value로 property name을 줬다’고 할 수 있다.

이 값은 문자열뿐만 아니라 불리언, 객체, 배열 등 모든 데이터타입을 넣을 수 있다.

```javascript
<Food favorite="kimchi" something={true} papapapa={['Hello', 1, 2, 3, 4]} />
```

이렇게 한 상태에서 누군가가 Food컴포넌트로 정보를 보내려고 하면, 리액트는 이 모든 키값들을 가져온다. 그리고 food 컴포넌트의 인자(argument)로 그것들을 넣는다. 실제로 인자로 받는 값을 콘솔에 찍어보면 컴포넌트에서 받고 있는 모든 데이터들이 나오고 object에 담겨있는걸 알 수 있다.

![React App - Chrome 2020-05-05 오후 8_51_28](https://user-images.githubusercontent.com/60246689/81065757-10f91300-8f17-11ea-823a-cda5eb3075ba.png)

```javascript
function Food(props){
  console.log(props)
  return <h1>I like potato</h1>
}

function App (){
  return <div>
    <Food favorite="kimchi" something={true} papapapa={["Hello", 1,2,3 ,4] } />
  </div>
```

해당 데이터들이 객체이기 때문에 객체안에 담긴 키를 인자로 전달하고, 그 인자를 받게끔 함수를 만들면 내가 원하는 데이터를 반환하는 컴포넌트를 만들수 있다.

혹은 객체 리터럴 안에 키(프로퍼티명)을 사용하는 방식 `{ favorite }` 도 동일한 결과를 얻을 수 있다.

```javascript
function Food(props) {
  console.log(props.favorite)
  return <h1>Hello world</h1>
} //결과 kimchi 출력
```

그래서 이 특징을 이용해서 앞서 봤던 Food 컴포넌트가 받게 되는 데이터에 따라 리턴하는 문장의 내용을 바꾸게 만들어보자.

```javascript
function Food({ favorite} ){
  return <h1>I like {favorite}</h1>
}

function App (){
  return <div>
    <Food favorite="kimchi"  />
    <Food favorite="fried_chicken"  />
    <Food favorite="dduckbokki"  />
  </div>

```

이 결과 내가 넣은 키값에 반응하는 컴포넌트가 만들어졌다.
완전 멋쪄... 리액트 이녀석...
