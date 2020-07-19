---
title: '[React Basis] 컴포넌트에 정보를 전달하기 2  feat. Array.map( )'
date: 2020-05-05 02:21:13
category: 'react'
draft: false
---

### Dynamic Component Generation

```javascript
  <Food favorite="kimchi"  />
  <Food favorite="fried_chicken"  />
  <Food favorite="ramen"  />
```

앞선 포스팅을 통해서 컴포넌트에 데이터를 전달하는 방법에 대해 알아봤다. 하지만 이걸로는 부족하다. <br>
왜냐면 앞으로 동적 웹을 만들것이기 때문에 서버에서 받게 되는 데이터가 어떤것이 될지 모른다는 상태에서 코드를 짜야하는데, 앞서 봤던 내용은 단순히 데이터를 새로 추가하는 정도밖에 지나지 않았으니까 말이다. 흐규...

그래서 이번 포스팅에서는 서버에서 데이터를 받는다는 가정을 하고,
어떤 데이터들을 받았을때 이걸 역동적으로 처리 할 수 있는 컴포넌트를 만들어보려고한다. 다음의 배열 데이터를 서버에서 받았다고 가정하고 보자.

```javascript
const foodILike = [
  {
    name: 'jjajangmyeon',
    image:
      'https://www.maangchi.com/wp-content/uploads/2007/07/jjajangmyeonAugflickr-75x75.jpg',
  },
  {
    name: 'dduckbokki',
    image:
      'https://www.maangchi.com/wp-content/uploads/2007/09/ddeokbokki_sizzle-75x75.jpg',
  },
  {
    name: 'fried-chicken',
    image:
      'https://www.maangchi.com/wp-content/uploads/2014/01/fried-chicken_basket-75x75.jpg',
  },
  {
    name: 'kimchi-pancake',
    image:
      'https://www.maangchi.com/wp-content/uploads/2015/05/kimchi-pancake-75x75.jpg',
  },
]
```

이 데이터를 통해 컴포넌트에서 object(객체)의 list를 어떻게 가져오는지 보게 될 건데, 그 방법은 바로 자바스크립트의 함수를 이용하는거다! <br>
사실 리액트는 자바스크립트이기 때문에 자바스크립트의 함수들을 그대로 사용할 수 있다. 이번 시간에 등장하는 함수는 `Array.map()`이다. 여기서 잠깐 개념을 정리하고 가자.

### Array.map() <br>

`Array.map()` 메서드는 배열 내의 모든 요소 각각에 대해 주어진 함수를 실행한 결과를 모아 새로운 배열을 반환한다. 배열 전체를 돌며 배열값을 사용해서 "또 다른 배열"을 만들고 싶을때 적합한데, 특히 원본 배열은 건들지 않고 그 값을 활용해서 새로운 배열을 만들어야 할 때 특히 유용하다.

다음 예시를 보면 쉽게 이해할 수 있다.

```javascript
const friends = ['jiwon', 'ho', 'jin', 'garam']

friends.map(friend => {
  console.log(friend + ' good')
})
```

![주석 2020-05-05 220304](https://user-images.githubusercontent.com/60246689/81069017-405e4e80-8f1c-11ea-8d2b-3c4f05e1b5b9.png)

### Array.map()을 활용한 컴포넌트 만들기

이제 이 map의 사용법을 알게 되었으니 배열을 활용해서 역동적으로 동작하는 컴포넌트를 만들어보자.

```javascript
function App() {
  return (
    <div>
      {foodILike.map(dish => (
        <Food name={dish.name} picture={dish.image} />
      ))}
    </div>
  )
}
```

위의 코드를 보면 foodILike라는 배열에 map 함수를 사용해서 각 요소마다 한번씩 돌아가는 함수를 실행시켰는데, 함수의 내용은 앞선 포스팅에서 배웠던 객체의 키로 접근하는 것과 크게 다르지 않다. <br>참고로 dish는 함수안에서 돌아가는 매개변수에 불과하다.

정리하면, 배열의 각 요소에서 name이라는 키값과 image라는 키값에 접근한다는 내용이다. 주의할점은 이 결과를 리액트는 객체로 리턴하기 때문에 객체 리터럴 `{ }`을 사용한다는 것이다. 객체 리터럴을 안쓰면 그냥 문자열이다 ~.~

그럼 이제 컴포넌트를 보자.

```javascript
function Food({ name, picture }) {
  return (
    <div>
      <h2> I like {name} </h2>
      <img src={picture} />
    </div>
  )
}
```

이 결과 실행된 화면을 보면, 리액트가 주어진 데이터에 반응하여 역동적으로 화면을 만들었다는걸 알 수 있다. <br>
잘했어 리액트~~

![주석 2020-05-05 221445](https://user-images.githubusercontent.com/60246689/81071205-51f52580-8f1f-11ea-93b1-2334623fcdcd.png)
