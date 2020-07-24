---
title: 'Redux 세션 내용 정리 '
date: 2020-06-12 01:00:03
draft: false
category: 'redux'
---

# Introduction to Redux

## 1. Intro

**redux의 키워드 : 전역 상태관리** <br />
store라는 중앙통제실을 만들어서 (여러가지 프로퍼티를 갖고 있는 객체)
state관리를 한번에 할 수 있게 하는데에 목적이 있다. <br />
이런 상태관리를 가장 유용하게 쓸 수 있는 대표적인 예시가 카트관리, 사이트의 언어관리(English, 한국어, 중국어 등) 이다.

Application의 전체 state는 store라고 불리는 곳에서 관리된다. store는 redux의 상태값(state)를 갖는 객체이다.

action은 state 변화를 일으킬 수 있는 하나의 현상이다. action을 dispatch(발행)해서 store에 저장하고, state가 변경되면 view에서 감지하게 된다. <br />
action은 사용자가 일으키는 이벤트라고 생각해도 되는데, view단에서 action이 일어날 수도 있다. <br />
view에서 action이 일어나면 -> 다시 dispatcher에 의해 store에 저장되고 -> state가 변경되면 -> 필요한 view에서 감지를 알아차린다.

위에서 언급한 개념을 간단하게 다시 정리하면 다음과 같다. (아래에서 좀 더 자세히 정리할 것임)

    - action: 상태 변화를 일으킬 수 있는 현상. 객체 형태를 취한다.
    - store: application의 전체 state를 가지고 있는 객체.
      참고로 app에는 단 하나의 store를 갖고 있는 것이 좋음.
    - view: 리액트에서는 component라고 생각하면 된다.

<br >

## 2. redux의 주요 개념

### 1. action

- state(상태) 변화를 일으킬 수 있는 하나의 현상. <br />
  상태에 어떠한 변화가 필요하게 될 땐, 액션이란 것을 발생시킨다. <br />
  예를 들어 사용자의 입력, 웹 요청 완료 등 <br />
  action은 객체 형태를 취하고, 액션의 행동을 설명하는 type 프로퍼티를 반드시 갖는다.

```jsx
action = {
    type: "ADD_MEMBER"
//액션이 어떤 행동을 하는지 정보. 리듀서를 분배할때 이걸 기준으로 나눈다.
    payload: {
        name: "sori"
    }
//액션으로 얼마를, 어떻게 취할 것인지에 대한 정보. 없어도 된다.
//카트의 수량을 변경하고싶다면 product id 같은걸 넘겨주면 된다.
}
```

- action creator: 액션을 생성하는 함수. 입력된 파라미터를 받아와서 액션 객체 형태로 만들어준다.

```jsx
function addTodo(data) {
  return {
    type: 'ADD_TODO',
    data,
  }
}

// 화살표 함수로도 만들 수 있음
const changeInput = text => ({
  type: 'CHANGE_INPUT',
  text,
})
```

<br >

### 2. reducer : 변화를 일으키는 함수. 두가지의 파라미터를 갖는다.

```jsx
function reducer(state, action) {
  // 상태 업데이트 로직
  return alteredState
}
```

- state를 실제적으로 변경하고, 이 변경된 값이 store 객체의 밸류가 되게 한다.
- store에 담겨있는 state 하나 하나는 각각의 다른 reducer로 연결이 된다.
- 보험회사의 보험접수팀, 인사, 재정팀 각각이 다른 reducer를 갖고 있어서 다른 로직을 처리해야 한다.
- reducer의 이름이 store의 키 값으로 그대로 들어간다.
- reducer에는 리셉션 같은게 있는데 rootReducer라고 부른다. 이 rootReducer는 최초로 액션을 받아서 모든 reducer에게 뿌려주는 역할을 한다. 그리고 이렇게 액션들을 받아서 각각 뿌려주는 과정을 dispatch라고 한다.

- 모든 reducer는 액션을 일단 다 받는다. 그후에 type을 이용해서 걸러서 자기에게 필요한 액션만 받는데,
  이때 사용하는 자바스크립트 문법은 switch case문이다. (각각의 타입별로 case를 나누어서 액션을 분배시킴).
  실행된 액션의 결과는 store에 저장된 state의 값으로 들어가는 방식으로 state가 업데이트 된다.

- switch문의 조건을 짤때 각각 액션을 인자로 받기 때문에 action.type 이런식으로 type에 접근해서 케이스별 조건문을 만들어줘야한다.
  각각의 reducer는 서로 다른 switch를 가지고 타입을 검사한다.

<br >

**[예시] 카트 구현** <br >
카트리스트라는 배열에 객체가 있을것이다.(수량, 컬러 등) <br >
근데 만약에 한 상품의 수량만 변경을 해주고싶다면,
이 상품을 담고 있는 배열을 찾아서 그 값만 업데이트 시켜줘야하는데 리덕스의 액션을 이용하면 좀 더 쉽게 바뀔 수 있다.

```jsx
 const number = (state=[], action) => {
     switch(action.type)
      case "ADD_NUMBER":
      return;
      case "MINUS_NUMBER" :
      return [...state, action.payload]
      default :
      return state;
 }
 //state가 최초 렌더 되었을때는 아무 값이 없기 때문에
 //최초의 값을 준다는 의미의 initialize를 해줘야한다.
 // 이후에 state가 정의된 값이 있다면 인자에 들어있는 값은 무시된다.
```

<br >

이런식으로 모든 reducer가 switch문으로 자기 타입에 해당하는 케이스의 로직을 내려받아서 수행한다.
디폴트에 있는애들은 모든애들이 기본적으로 수행하고 있어야하는 베이스 행동을 정의해놓는다.

**[참고]** spread operator <br >
기존의 배열 모든 요소를 그대로 복사.

```jsx
const arr = [1, 2, 3, 4, 5]

const hello = [...arr]

arr === hello //false
//배열도 결국 객체의 일부여서, 객체의 특성을 그대로 가져간다.
//실제 레퍼런스주소가 다르기 때문에 같은 값을 가진 배열들이어도
//실제로는 다르다. 내부의 값을 직접 접근하면 true.
// 이 개념이 리덕스 할 때 너무 중요함!

const obj = {
  a: 10,
  b: 20,
  c: 30,
}

const newObj = { ...obj, b: 14 }
//기존의 값을 그대로 가져오고 b값만 업데이트
//카트에서 수량만 바꾸고싶을때 이런걸 사용하면 된다. reducer의 주된 방식
```

<br>

3. dispatch

   - 각자 맡은 위치, 역할에 보내준다는 의미.

<br>
<br>

## 3. Redux의 특징

리덕스의 3가지 특징이 있다. [공식문서]('https://redux.js.org/introduction/three-principles')

이 중에서 알고 넘어갈 건 다음과 같다.

- State는 읽기 전용이다. 컴포넌트에서는 state를 참조만 할 수 있고, (setState처럼) 변경할 수는 없다. state를 변경하고 싶다면, Action을 dispatch(발행)해서 app의 state를 변경해야만 한다. 이렇게 해야 데이터의 흐름이 한 방향으로만 흐르게 된다.

- reducer는 pure function(순수 함수)으로 이뤄져야 한다. 순수 함수란 같은 입력 값을 넣으면 같은 출력 값을 내는 함수를 의미한다. reducer는 이전의 state와 action을 입력 받고, 이로 인해 변화한(next) state를 return한다. 즉, reducer는 action에 의해 state를 변경시키는 함수라고 생각하면 된다.

<br >

## 4. react-redux

### connect([...options])(Counter)

컴포넌트를 리덕스에 연결하는 또다른 함수를 반환한다.
즉, store에 연결된 새로운 컴포넌트 클래스가 반환되고, 만약 옵션이 없으면 this.props.store로 접근 가능하다.
여기에 해당하는 옵션은 mapStatetoProps처럼 state값을 props로 전달해주는것 같은 경우를 말한다.
<br >
<br >
