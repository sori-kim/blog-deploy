---
title: "[TypeScript] 타입스크립트 시작하기 & Basis"
date: "2020-06-30T02:00:03.284Z"
template: "post"
draft: false
slug: "typescript/200630"
category: "typescript"
tags:
  - "typescript"
description: "타입스크립트와 친해지길 바라 프로젝트... Let's get started 🤘🏻"
---

# 1. Intro & 시작하기

## 왜 타입스크립트?

타입스크립트를 한마디로 정의하면 정적 타이핑을 지원해주는 컴파일러이다.  
여기서 정적 타이핑이란 **자바스크립트의 변수, 함수의 매개변수, 함수 리턴값의 타입을 명시화**하는 것으로,
타입 분석을 프로그램을 실행시켜보지 않고 런타임 이전에 진행하여 오류를 줄일 수 있도록 도와준다.

## 타입스크립트 시작하기

- 내 프로젝트에 타입스크립트 패키지 설치하기

```
$ yarn add typescript # 또는 npm install --save typescript
```

그 다음에는 package.json 파일을 열어서 다음과 같이 build 스크립트를 만들고

```jsx
{
  "name": "ts-practice",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "dependencies": {
    "ts-node": "^8.4.1",
    "typescript": "^3.6.3"
  },
  "scripts": {
    "build": "tsc"
  }
}
```

추후 빌드를 할 때 yarn build 라고 입력하면 된다. (또는 npm run build)

# 2. Basic types

### Basic types의 예시 한 눈에 보기

<br>

<img width="726" alt="스크린샷 2020-06-30 오후 2 54 52" src="https://user-images.githubusercontent.com/60246689/86105810-c4871980-bafa-11ea-8bfd-97f01e5e13f6.png">

<br>

### 차근 차근 살펴보기

타입스크립트에서 타입을 정의할때는 암시적 혹은 명시적인 방법을 사용할 수 있다.  
암시적인 방법은 말 그대로 특정 타입의 데이터를 변수에 선언하면 암시적으로 타입이 정의된다.
변수를 선언할때 처음 정의된 데이터타입이 아닌 다른 타입의 값은 들어올 수 없게 된다.

```jsx
let character = "mario";
let age = 30;
let isBlackBelt = false;

character = "rosie";
age = 40;
```

<br>

한편, 명시적인 방법은 직접 타입을 명시화하는 것이다.  
변수를 선언하면서 콜론과 함께 데이터 타입을 명시해주면 된다.  
함수 파라미터의 경우, 인자로 들어올 수 있는 데이터의 타입을 파라미터와 함께 정의하고 있다.  
만약 정의된 타입 이외의 타입값이 들어오면 타입스크립트에서 자바스크립트로 컴파일 자체가 되지 않는다.

```jsx
let character: string;
let age: number;
let isLoggedIn: boolean;

age = 30;
isLoggedIn = true;

const circ = (diameter: number) => {
  return diameter * Math.PI;
};

console.log(circ("hello")); //에러!
console.log(circ(7.5)); //23.561944901923447
```

<br>

### Arrays & Objects

배열과 객체에서의 타입 정의 역시 위에서 살펴본 방식과 동일하게 동작하는데, 즉 한번 정의내린 타입은 바꿀 수 없다.

#### Array

```jsx
let names = ["hyo", "daniel", "clark"];
names.push("jerry");

console.log(names);
//["hyo", "daniel", "clark", "jerry"]
names.push(3); //에러
```

만약 배열의 요소에 한번이라도 정의된 타입의 경우, 해당 타입의 다른 값을 추가 할 수 있지만
그렇지 않은 경우 해당 타입의 새로운 값을 추가할 수 없다. (undefined, null 제외)

```jsx
let mixed = ["ken", "ruka", 10, "rosie", 23];

mixed.push("daniel");
mixed.push(undefined);
mixed.push(null);
mixed.push(true); //에러
```

<br>

#### Object

객체의 경우 최초의 정의내린 구조의 데이터타입, 형식이 동일하지 않는 경우 그 값을 사용할 수 없다.

```jsx
let rimu = {
  name: "rosie",
  age: 5,
  isHuman: true,
};

rimu.age = 40;
rimu.name = "sori";
rimu.favorite = "eat"; //에러. favorite 프로퍼티는 처음보는애다! isHuman은 어디갔냐?

rimu = {
  name: "sori",
  age: 100,
};
//에러. isHuman 프로퍼티가 없다!
```

객체의 경우, 특히 처음에 정의된 값과 정확히 동일한 구조 + 동일한 데이터타입을 가지고 있을 경우에만 값이 적용될 수 있다.

<br>

### 타입의 명시적 선언(Explicit Types), union Types

#### array

```jsx
let ninjas: string[] = [];
//최초의 빈배열을 선언하고, 타입을 string으로 한정했다.

let mixed: (string | number | boolean)[] = [];
mixed.push("hello");
mixed.push(2);
mixed.push(false);
//타입이 string 또는 Number 또는 boolean으로 한정되었다.
```

#### object

```jsx
let ninjaOne: object;
ninjaOne = { name: "yoshi", age: 30 };

let ninjaTwo: {
  name: string,
  age: number,
  beltColor: string,
};

ninjaTwo = { name: "rosie", age: 55, beltColor: "black" };
```

### any

any는 어떤 타입을 사용해야할지 확실히 모를때 사용한다.

```jsx
let mixed: any[] = [];
mixed.push(5);
mixed.push("mario");
mixed.push(false);
console.log(mixed);
//[5, 'mario', false];
```

```jsx
let ninja: { name: any, age: any };
ninja = { name: "yoshi", age: 25 };
```

하지만 any를 남발해서 사용할 경우 타입스크립트의 본질을 흐트러트릴 수 있기 때문에 주의해서 사용해야한다.
주로 어떠한 타입으로 데이터가 들어올지 불확실한 경우, 예를들면 user input 이나 3rd party에서 받는 값에서나 Any를 쓰도록 하자.

<br>

_이글은 The Net Ninja의 [youtube]('/')와 velopert님의 typescript tutorial을 학습하며 정리한 포스팅입니다._
