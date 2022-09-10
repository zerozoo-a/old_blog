---
layout: post
title: React element를 함수형스럽게 다루기_1
date: 2022-09-10 17:02 +0900
categories: [JS, FS]
---

## 출처 
이 포스트는
[Jack Hsu](https://jaysoo.ca/2017/04/30/learn-fp-with-react-part-1/)
님의 블로그와 [itchallenger](https://itchallenger.tistory.com/m/543)님의 블로그를 보고
저의 해석을 붙이며 만든 포스트입니다.

---

인터넷을 돌아다니던 중 제가 react를 사용하면서 늘 고민이던 부분을 시원하게 그리고 너무나도 잘
설명해준 포스트를 만났습니다. 그게 바로 위 출처입니다. 감사합니다.

---

react의 함수형 component를 사용하다 보면 이런 의문이 들고는 했습니다.
함수형 component이지만 좀 더 함수형스러운 사용법을 취할 수는 없는 것인가?


예를 들어 이런 것입니다.

```js
go(range(1,10),
    odd,
    sum,)
```
위와 같은 함수형 thread를 작성하듯 dom을 다루는 것도 이렇게 다루는게 가능하지 않을까?
라는 생각입니다.

서론이 너무 길어져 바로 코드를 보시는게 좋겠습니다.

제가 바라는 결과물은 간단합니다.
```js
const hello = ({name}) => <span>{name}</span>
const li = (element)=><li>{element}</li>
const ul = (element)=><ul>{element}</ul>
```
위와 같은 함수를 연속적으로 호출하여 `ul > li > hello`의 자식 관계를 가지는 dom을 그려내면 되는 것이죠.

이제 위의 선지자이신 Jack님의 코드를 보며 작성한 코드는 아래와 같습니다.

```js
const View = (computation) => ({
    fold: computation,
    map:(wrapper)=>View((props) => wrapper(computation(props)))
})

const foo = ({foo})=><div>foo: {foo}</div>
const li = (element) => <li>li: {element}</li>
const ul = (element) => <ul>ul: {element}</ul>
const test = View(foo)
    .map(li)
    .map(ul)
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
      {test.fold({foo:"bar"})}
  </React.StrictMode>
);
```

View 함수를 자세하게 이해하면 어려운 부분은 아마 없을 것 같습니다.


1. View 함수는 함수를 인자로 받습니다.
2. View 함수의 map 메소드는 다시 View 함수를 호출하여 map 함수의 인자인 함수로
computation 값 즉 두 번째로 호출 된 View 함수는 바로 직전 함수의 인자값을 (여기에서는 첫번째 호출시 받은 함수)
를 호출하는 함수를 인자로 넘겨줍니다.


---
### 이게 대체 무슨 말인가 🤷

처음 View의 인자로 들어간 함수를 a라고 합시다.
map 함수에 인자로 넣어준 함수를 b라고 합시다.

map 함수가 반환하는 것은
`View((props)=>b(a))` 이다.
이제 알맹이가 없는 상태의 함수들의 연속적인 호출만 정의 되었다.


```js
(props)=><li><span>{name}</span></li>
```

자 여기서 한 번 더 map을 호출하면

```js
(props)=><ul><li><span>{name}</span></li></ul>
```

wrapper는 계속 중첩되어도 상관 없다.

이제 알맹이만 넣어주면 된다.

```js
test.fold({foo:"bar"})
```

fold에 저장해둔 computation 함수(a)를 실행시킴으로써 그 순간부터 나머지 함수들도
연속적으로 실행이 되게 된다.












