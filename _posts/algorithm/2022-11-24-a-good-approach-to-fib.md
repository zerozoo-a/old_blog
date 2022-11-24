---
layout: post
title: 피보나치 수열을 구현하는 여러 방법
date: 2022-11-24 01:33 +0900
---

# 피보나치 수열

---

## 📇 Index


- [피보나치 수열](#피보나치-수열)
  - [📇 Index](#-index)
  - [가장 단순한 형태](#가장-단순한-형태)
  - [memo와 helper 함수의 효용성](#memo와-helper-함수의-효용성)
  - [bottom-up 방식](#bottom-up-방식)
  - [상태압축을 통한 공간최적화](#상태압축을-통한-공간최적화)
- [🏕️️](#️️)
  

---
<br>

## 가장 단순한 형태 
---

```js
/**
 * 가장 간단한 형태의 피보나치 수열 문제 풀이
 *
 * 수행 속도: O(n^2)
 *
 * @param {number} n
 * @returns {number}
 */
function simple_fib(N) {
  if (N < 2) return N;
  if (N === 2) return 1;

  return simple_fib(N - 1) + simple_fib(N - 2);
}
```
가장 단순한 형태의 풀이로 반복되는 덧셈과 트리 큰 값의 저장을 하지 않기 때문에 속도는 매우 느리다.

<br>
<br>
## memo와 helper 함수의 효용성
---

```js
/**
 * helper 함수를 통해 O(n) 만큼의 공간복잡도를 사용하여
 * 시간복잡도를 O(n)까지 끌어내렸다.
 * 
 * @param {number} N
 * @returns {number}
 */
function memo_fib(N) {
  if (N === 0) return 0;
  const memo = new Array(N);
  return helper(memo, N);
}

/**
 *
 * @param {&Array<number>} memo
 * @param {number} N
 * @return {number}
 */
function helper(memo, N) {
  if (N === 1 || N === 2) return 1;
  if (memo[N] !== undefined) return memo[N];

  memo[N] = helper(memo, N - 1) + helper(memo, N - 2);
  return memo[N];
}
```

helper 함수는 memo 배열을 통해 배열 index와 **N**을 1 대 1 매칭하여 값을 저장하므로 속도는 O(n), 공간은 O(n)이 사용된다.

자바스크립트의 배열은 일반 언어의 배열과는 다른 형태로 데이터를 관리한다. `c++`의 `vector`와 조금 닮은 구석이 있음, 그러므로 배열의 크기를 미리 할당하는 것과 하지 않는 것에는 차이가 있다고 생각한다.

<br>
<br>
## bottom-up 방식

```js
/**
 * @param {number} N
 * @return {number}
 */
function fib_bottom_up(N) {
  if (N === 0) return 0;
  if (N === 1 || N === 2) return 1;
  const dp = new Array(N);
  dp[1] = dp[2] = 1;

  for (let i = 3; i <= N; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
  }
  return dp[N];
}
```
JS는 콜 스택의 양도 적다. 그래서 이 콜 스택 관리를 못하면 터지게 되는데 bottom-up 방식은 재귀가 아닌 반복문을 통해 `1, 1, 2, 3, 5 ...` 이런식으로 직접 사람 손을 가지고
피보나치 수열을 계산하는 듯한 접근 방식으로 콜스택이 쌓이지 않는다. 즉 스택으로부터 자유롭다.


<br>
<br>
## 상태압축을 통한 공간최적화 

```js
/**
 * 상태 압축을 통해 공간 복잡도 O(n)에서 O(2)로 줄어들었다.
 *
 * @param {number} N
 * @return {number}
 */
function fib_pressed(N) {
  if (N === 0) return 0;
  if (N === 2 || N === 1) return 1;

  let prev = 1,
    curr = 1,
    i = 3;

  for (; i <= N; i++) {
    const sum = prev + curr;
    prev = curr;
    curr = sum;
  }

  return curr;
}
```

사실 N 번째의 피보나치 수열의 값을 알아내는데 모든 값을 메모 할 필요는 없다.

`1, 1, 2, 3, 5 ...`을 구할 때 필요한 값은 `2, 3`이다.
이를 착안한게 값을 계속 덮어 씌우는 방식이다. 바로 위의 코드가 그것의 구현이다.


--- 

요즘 백준 문제 풀이를 계속 올리다가,
백준 문제 풀이만을 위한 블로그가 되는 것 같기도 하고

더 빠르고 아름다운 백준 풀이를 만들기 위해 다시 cs, algorithm, data-structure 에 대해 탐구하는 시간을 가지고자 합니다. 

더 멀리, 더 정확히 가기 위한 시간이라고 생각하겠습니다.

# 🏕️️
