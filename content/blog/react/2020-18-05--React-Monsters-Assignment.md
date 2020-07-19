---
title: '[React Basis] Session #3. React Props & States / Array.map() 활용 - Monsters 🧟‍♂️ '
date: '2020-05-17T10:12:03.284Z'
template: 'post'
draft: false
slug: 'react/200519'
category: 'react'
tags:
  - 'react'
description: ''
---

## Goal

✔️ state의 개념에 대해 한 문장으로 설명할 수 있다. <br>
✔️ 부모 요소의 state 데이터를 자식 요소에서 반영시킬 수 있다. <br>
✔️ 이벤트를 통해 state 데이터를 바꿀 수 있다. <br>
✔️ props의 개념에 대해 한 문장으로 설명할 수 있다. <br>
✔️ 부모 state - 자식의 props - 자식 component 어떻게 연결되는지 명확히 이해한다. <br>
✔️ props 개념을 활용하여 자식 요소에서 일어난 이벤트로 부모의 state 값을 바꿀 수 있다.<br>

# 1. States & Props of Component

## states

State는 컴포넌트 내부에서 정의하는 컴포넌트의 상태값을 말하는데, 객체 형식으로 되어 있다. 항상 디폴트 값을 지정해줘야한다.
<br>
state는 **고정적인 데이터가 아니라 변경될 수 있는 데이터를 처리할 때** 주로 사용하는데, 그 이유는 리액트가 기본적으로 상태 변화를 계속적으로 감지하기 때문에 화면을 한번 그려준 후 state의 값을 변경하면 그 값을 자동으로 감지한 후 render() 함수를 실행해서 화면에 적용을 해주기 때문이다. 그리고 state는 props보다 좀 더 지능적으로 동작하기 때문에 함수형 컴포넌트가 아닌, 클래스형 컴포넌트를 사용한다.

state를 이용하는 클래스형 컴포넌트를 사용할때는 다음의 방법을 지키면 된다.

1. state를 사용할때는 먼저 constructor함수 안에 state의 기본값을 정의해야한다. 이때 props를 매개변수로 보내고, super()가 이 props를 전달받아서 props를 초기화 해주어야 한다.

2. 그리고 render() 함수를 정의하고, 그 안에 return문을 작성한다.

```jsx
class Login extends React.Component {
 constructor(){ //디폴트값 지정하기
     super();
     this.state = {
         imgList = [],
         fontColor: true,
         fontSize: "15px"
     }
 }

 handleColor = () => {
     this.setState({
         fontSize: "100px"
         fontColor: !fontColor;
     })
 }

 render(){
return (
    <div className={this.state.fontColor ? "login" : "logout"}>

        <button onClick={this.handleColor}>로그인</button>
        <p style={{fontColor: this.state.fontColor ? "blue" : "red" }}>Change Color</p>
     </div>
  )
 }
}

```

<br>

- 이벤트를 발생시키는 요소와 이벤트 결과로 바뀌어지는 요소는 별개라는것을 인지하고 있는게 중요하다.
- 삼항연산자를 이용해서 클래스이름을 토글로 바꿀 수 있다. 인라인스타일링은 최대한 지양하는게 좋기 때문에 css파일에 특정 클래스로 스타일링을 지정해 놓은 뒤, 조건에 따라 클래스 이름이 추가되었다가 삭제되는 방식을 활용하자.

- 참고로 화살표 함수 쓰면 bind를 안해도 된다.
- 이벤트 함수에서 ()는 바로 호출을 의미하기 때문에 쓰지 않아야하지만, 자식요소로 인자를 넘겨줘야할때는 괄호를 써야한다. handleClick(num) 이런 식으로... 이 방식은 굉장히 많이 쓰이는데, 이벤트함수 호출할때 setState의 값만 다르게 넘겨주는 방식으로 주로 쓰인다.

  <br>

## props

리액트는 props를 입력으로 받아서 리액트 요소를 반환하는 형태로 동작한다.<br> 여기서 props는 속성(property)의 약자이다.
props는 어떤 컴포넌트 내부에 다른 컴포넌트가 있을때 부모컴포트에 있는 state의 데이터를 자식 컴포넌트에 전달하기 위해 사용한다.
주로 고정적인 데이터를 전달할때 사용한다.

```jsx
class LoginChild {
    <div style={{backColor: this.props.backColor}}>
    </div>
 }
```

```jsx
<LoginChild backColor={this.state.childBackColor} />
```

<br>

props를 이용하면 부모에 있는 이벤트함수도 자식에게 전달할 수 있다.
부모 컴포넌트에 자식 컴포넌트를 어펜드해줄때 변수 = {전달하고싶은값} 형식으로 보내고,
내려받는 자식 컴포넌트에서는 this.props.변수 로 받는다.

혹은 부모에서 자식으로 값을 줄때 아예 값만 다르게 여러번 보내줄 수도 있다.
자식은 그 값이 필요한 곳에서 props로 받기만 하면 되니깐.

참고로, keydown 이벤트같은건 onChange로 하면 된다.

```jsx
handleInput = event => {
  console.log(event.target.value)
  this.setState(
    {
      id: event.target.value,
    },
    () => console.log(this.state.id)
    //자바스크립트는 바뀐 스테이트값이 아니라 그 이전에 값이 찍히는데,
    //그걸 방지하기 위해서 콜백함수로 확인을 해봐야한다.
  )
}
```

# 2. Array.map()

map은 javascript의 배열을 다루는 메서드로, 배열의 요소를 돌면서 요소 하나 하나를 처리한다.

주의할 점은 react에서 map을 이용할때는 key라는 고유값을 부여해줘야지 각각의 배열이 구분되어 리액트의 실행속도가 빨라진다. <br>
이 때 key로 사용할 수 있는 값은 배열의 index값이나 그 외에 고유한 값으로 사용할 수 있는 그 외의 모든 값들이 해당된다.

```jsx
const arr = [1, 2, 3, 4, 5]

arr.map((num, index) => {
  return <Component key={index} number={num} /> // 새로운 배열을 리턴한다.
})

//<Component number={1}/>
//<Component number={2}/>
//<Component number={3}/>
//<Component number={4}/>
//<Component number={5}/>
```

```jsx
 render(){
     return (
         <div className="App">
         {this.state.arr.map((obj)=>{
             <Component name={obj.name} age={obj.age} />
         })}
         </div>
     )
 }
```

## Array.map()을 활용하여 컴포넌트 재사용하기

(1)fetch를 이용해서 외부 api의 데이터를 받아온 후 (2) 부모컴포넌트와 자식컴포넌트 구조를 활용해서 카드를 만들고, <br>
(3) 검색창에 입력한 스펠링으로 몬스터들이 필터링되는 구조의 웹앱을 만들어보는 과제이다.

<img width="582" alt="스크린샷 2020-05-24 오후 4 25 47" src="https://user-images.githubusercontent.com/60246689/82748222-463ca680-9ddb-11ea-848d-6a0ac94a4d8a.png">

<br>

### (1) fetch를 이용해서 api호출

```jsx
//App.js
class App extends Component {
  state = {
    monsters: [],
    userInput: "",
  };

  // 데이터 로딩
  componentDidMount() {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((res) => res.json())
      .then((res) => this.setState({ monsters: res }));
  }
```

컴포넌트의 마운팅이 끝났을때 api를 호출해서 데이터를 로딩해오고, 이 데이터를 monsters라는 state에 집어넣었다.

### (2) 부모컴포넌트와 자식 컴포넌트 구조를 활용해서 카드를 만든다.

1. App.js

- 최상위 부모
- 컴포넌트가 마운트되면 api로부터 데이터를 로딩해온다.

```jsx
render() {
    // 필터링 로직
    const { monsters } = this.state;

    return (
   <div className="App">
        <h1>컴포넌트 재사용 연습!</h1>
        {/* <SearchBox handleChange=정의한메소드 /> */}
        <CardList props={this.state.monsters} />
      </div>
    );
  }
  export default App;
```

<br>

2. CardList.js

- Card 컴포넌트를 품고있는 부모이자 App 컴포넌트의 자식.
- App에서 최초로 받은 monsters 데이터를 내려받아서 다시 Card 컴포넌트로 보낸다.

```jsx
class CardList extends Component {
  render(props) {
    const monsters = this.props.monsters

    return (
      <div className="card-list">
        {monsters.map(monster => {
          return (
            <Card
              key={monster.id}
              id={monster.id}
              name={monster.name}
              email={monster.email}
            />
          )
        })}
      </div>
    )
  }
}

export default CardList
```

<br>

3. Card.js

   - 나는 카드에요! (카드를 화면에 렌더링 해주는 역할만 담당함)
   - 이때 부모의 state를 props로 받아오는데, 속성의 이름은 부모(CardList)에서 결정한다.

```jsx
class Card extends Component {
  render() {
    return (
      <div className="card-container">
        <img
          src={`https://robohash.org/${this.props.id}?set=set2&size=180x180`}
          alt={this.props.name}
        />
        <h2>{this.props.name}</h2>
        <p>{this.props.email}</p>
      </div>
    )
  }
}
export default Card
```

### (3) 검색창에 입력한 스펠링으로 몬스터들이 필터링되게 한다.

검색 필터링 기능의 경우, 먼저 <SearchBox />라는 검색창 컴포넌트를 삽입한 후 onChange 이벤트에 반응하는 핸들러 함수를 정의하여 props로 전달을 한다. <br>
그리고 그리고 filter 함수를 사용하여 monsters 배열의 각 요소마다 name이 입력한 값을 갖고 있는 배열을 리턴하게 만들었다. <br>
아무것도 입력한 값이 없을때는 모든 요소들이 필터에 해당되기때문에 모든 요소들이 화면에 나온다. <br>
주의할 점은, 항상 어떤 두 값을 비교할때는 조건을 동일하게 해줘야하기 때문에 monsters 요소의 이름들과 입력하는 값 양쪽을 소문자로 변환해서 비교해야한다는 점이다.

App.js

```jsx

  state = {
    monsters: [],
    userInput: "",
  };

  getInputValue = (event) => {
    this.setState({
      userInput: event.target.value,
    });
  };


render() {
    // 필터링 로직
    const { monsters, userInput } = this.state;
    const filtered = monsters.filter((monster) => {
      return monster.name.toLowerCase().includes(userInput.toLowerCase());
    });

    return (
      <div className="App">
        <h1>컴포넌트 재사용 연습!</h1>

        <SearchBox
          handleChange={this.getInputValue}
          userInput={this.state.userInput}
        />
        <CardList monsters={filtered} />
      </div>
    );
  }

```
