---
title: '[TypeScript] 함수에서 타입 사용하기'
date: 2020-06-30 03:00:03
draft: false
category: 'typescript'
---

## 함수에서 타입 사용하기

기본적으로 함수를 정의할때에는 `let a: Function;` 이라고 타입을 선언한 뒤,  
함수의 내용을 밑에 정의한다.

함수의 파라미터에 타입을 지정하는것은 앞선 포스팅에서 다루었는데,  
옵션으로 하나의 파라미터를 더 주고싶다면 파라미터 이름 뒤에 ?를 붙인다.  
만약 옵션 타입을 지정해놓고 값을 입력하지 않으면,  
해당 파라미터는 undefinded가 나온다.  
참고로 옵셔널 파라미터는 항상 가장 마지막에 작성하는것이 코드의 가독성에 좋다.

```jsx
let greet: Function

greet = () => {
  console.log('hello, again')
}

const add = (a: number, b: number, c?: number | string) => {
  console.log(a + b)
  console.log(c)
}

add(5, 10)
// 15, undefined
// 옵셔널파라미터에 아무값을 주지 않으면 undefined
```

파라미터의 디폴트 값을 지정해주고싶을때는 다음과 같이 쓴다.

```jsx
const add = (a: number, b: number, c: number | string = 10) => {
  console.log(a + b)
  console.log(c)
}
```

<br>

## 타입스크립트와 함수의 Return

함수의 리턴은 기본적으로 파라미터로 지정된 값들을 그대로 반환한다.  
그래서 리턴 값의 타입을 설정해주지 않아도 number와 number의 합은 number를 리턴한다.

```jsx
const minus = (a: number, b: number) => {
  return a + b
}

let result = minus(10, 7)
// 3
```

만약 명시적으로 함수의 반환하는 값을 정의하고싶다면 다음과 같이 작성할 수도 있다.

```jsx
const minus = (a: number, b: number): number => {
  return a + b
}
```

<br>
<br>

한 가지 예를 더 보자.

```jsx
function sum(x: number, y: number): number {
  return x + y
}

sum(10, 5)
```

타입스크립트를 사용하면, 코드를 작성하는 과정에서 함수의 파라미터로 어떤 타입을 넣어야하는지 바로 알 수 있다.  
<img width="703" alt="스크린샷 2020-06-30 오후 6 15 38" src="https://user-images.githubusercontent.com/60246689/86108136-c0a8c680-bafd-11ea-96ea-1dff7a9dea38.png">

또, 위의 코드 : number는 해당 함수의 결과물이 숫자라는걸 명시하고 있다.  
만약 이때 return 값으로 null 같은걸 넣으면 당연히 에러가 나온다.

<br>

## 아무것도 리턴하지 않는 함수: void

```jsx
function returnNothing(): void {
  console.log('I am just saying hello world')
} //함수에서 아무것도 반환하지 않아야 한다면 반환타입을 void로 설정하기
```

만약 함수가 리턴을 하고 있지않다면? <br>
console.log의 동작만 실행하는 함수의 경우 아무것도 리턴을 하고 있지 않는다.  
사실 이 함수도 무언가 값을 반환하고 있다고 볼 수 있는데,  
이때 반환하고 있는 값을 **void**라고 부른다.

실제로 마우스를 대보면 void를 리턴하고 있다고 나오기 때문에 사실상 아래와 같이 쓸 수 있다.

```jsx
const add = (a: number, b: number, c: number | string = 10): void => {
  console.log(a + b)
  console.log(c)
}
```

<br>

주의할 점은, 타입스크립트에서 void와 undefined를 엄격하게 구분하고 있다는 것이다.  
undefinde는 우리가 옵셔널 파라미터를 주고 아무런 값을 입력하지 않았을때의 결과 같은 거라면,  
void는 아무값도 리턴하지 않는 경우를 의미한다.

<br>

<br>

_이글은 The Net Ninja의 [youtube]('/')와 velopert님의 typescript tutorial을 학습하며 정리한 포스팅입니다._
