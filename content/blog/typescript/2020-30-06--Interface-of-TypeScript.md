---
title: '[TypeScript] Interface & Type Alias 사용해보기  '
date: 2020-06-30 04:00:03
draft: false
category: 'typescript'
---

# 1.Interface 사용해보기

`interface`는 클래스 또는 객체를 위한 타입을 지정할때 사용되는 문법이다.

## 클래스에서 interface 를 implements 하기

우리가 클래스를 만들 때, 특정 조건을 준수해야 함을 명시하고 싶다면  
interface 를 사용하여 클래스가 가지고 있어야 할 요구사항을 설정하면 된다.  
그리고 클래스를 선언 할 때 implements 키워드를 사용하여  
해당 클래스가 특정 interace의 요구사항을 구현한다는 것을 명시적으로 말해줘야한다.

```jsx
interface Shape {
  getArea(): number;
  //Shape interface에는 getArea라는 함수가 꼭 있어야 하며,
  //해당 함수의 반환값은 숫자
}

class Circle implements Shape {
  //`implements` 키워드로 해당 클래스가
  //Shape interface의 조건을 충족하겠다는 것을 명시
  radius: number

  constructor(radius: number) {
    this.radius = radius
  }
  //너비를 가져오는 함수 구현
  getArea() {
    return this.radius * this.radius * Math.PI
  }
}

class Rectangle implements Shape {
  width: number
  height: number
  constructor(width: number, height: number) {
    this.width = width
    this.height = height
  }
  getArea() {
    return (this.width = this.height)
  }
}

const shapes: Shape[] = [new Circle(5), new Rectangle(10, 5)]

shapes.forEach(shape => {
  console.log(shape.getArea())
})
//78.53981633974483
//5
```

여기까지 코드를 작성하고 yarn build 명령어를 실행.  
그 다음엔, node dist/practice 명령어를 입력하여  
컴파일된 스크립트를 실행시켜보면 원하는 동작이 잘 나온걸 확인 할 수 있다.

<br>

## public, private accessor

만약 constructor를 선언할때 파라미터 쪽에 public 또는 private 같은 접근자를 사용하면  
직접 하나 하나 설정해주는 작업을 생략할 수 있다.

```jsx
// Shape 라는 interface 를 선언
interface Shape {
  getArea(): number;
  // Shape interface 에는 getArea 라는 함수가 꼭 있어야 하며 해당 함수의 반환값은 숫자
}

class Circle implements Shape {
  //`implements` 키워드로 해당 클래스가
  //Shape interface의 조건을 충족하겠다는 것을 명시

  constructor(public radius: number) {
    this.radius = radius;
  }
  // 너비를 가져오는 함수를 구현
  getArea() {
    return this.radius * this.radius * Math.PI;
  }
}

class Rectangle implements Shape {
  constructor(private width: number, private height: number) {
    this.width = width;
    this.height = height;
  }
  getArea() {
    return this.width * this.height;
  }
}

const circle = new Circle(5);
const rectangle = new Rectangle(10, 5);

console.log(circle.radius); //  radius는 public이기 때문에 에러 없이 작동
console.log(rectangle.width); // width는 private 이기 때문에 에러 발생!

const shapes: Shape[] = [new Circle(5), new Rectangle(10, 5)];

shapes.forEach(shape => {
  console.log(shape.getArea());
});
```

<br>

`public` accessor는 특정 값이 클래스의 코드 밖에서도 조회 가능하다는 것을 의미한다.  
예를 들어서 circle.width 이런 식으로 코드를 작성하면 해당 값을 바로 조회 할 수 있다.  
반면에 `private` accessor는 특정 값을 클래스의 코드 밖에서 조회 할 수 없다는 것을 의미한다.
그래서 rectangle.width 를 조회 하려고 하면 컴파일 단계에서 에러가 발생해버린다.

<br>

## 일반 객체를 interface로 타입 설정하기

```jsx
interface Person {
  name: string;
  age?: number;
  // 설정을 해도 되고 안해도 되는 값
}
interface Developer {
  name: string;
  age?: number;
  skills: string[];
}

const person: Person = {
  name: '김사람',
  age: 20,
}

const expert: Developer = {
  name: '김개발',
  skills: ['javascript', 'react'],
}
```

지금 보면 Person 과 Developer 가 형태가 유사하다.  
이럴 땐 interface 를 선언 할 때 다른 interface 를 extends 해서 상속받을 수 있다.

```jsx
interface Person {
  name: string;
  age?: number;
}
interface Developer extends Person {
  skills: string[];
}

const person: Person = {
  name: '김사람',
  age: 20,
}

const expert: Developer = {
  name: '김개발',
  skills: ['javascript', 'react'],
}

const people: Person[] = [person, expert]
```

# 2. Type Alias 사용하기 (타입의 변수화)

type이 같은 형식으로 반복된다면 변수로 지정해서 코드를 단축할 수 있다.

```jsx
type Person = {
  name: string,
  age?: number,
}

// & 는 두개 이상의 타입들을 합칠때
type Developer = Person & {
  skills: string[],
}

const person: Person = {
  name: '김사람',
}

const expert: Developer = {
  name: '김개발',
  skills: ['javascript', 'react'],
}

type People = Person[] // Person[] 를 이제 앞으로 People 이라는 타입으로 사용 할 수 있음
const people: People = [person, expert]

type Color = 'red' | 'orange' | 'yellow'
const color: Color = 'red'
const colors: Color[] = ['red', 'orange']
```

## type과 interface ... 뭘 써야할까?

type 과 interface 를 배웠는데, 어떤 용도로 사용을 해야 할까?  
결론은 무엇이든 써도 상관 없는데 일관성 있게만 쓰면 된다.  
다만 라이브러리를 작성하거나 다른 라이브러리를 위한 타입 지원 파일을 작성하게 될 때는  
interface를 사용하는것이 권장 되고 있다.

이에 대해 더 공부해보고싶다면 아래 링크의 문서들를 읽어보기.

- https://medium.com/@martin_hotell/interface-vs-type-alias-in-typescript-2-7-2a8f1777af4c
- https://joonsungum.github.io/post/2019-02-25-typescript-interface-and-type-alias/
