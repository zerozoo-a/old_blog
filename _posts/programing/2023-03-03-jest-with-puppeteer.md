---
layout: post
title: jest-with-puppeteer
date: 2023-03-03 00:00 +0900
categories: [js, test]
---
jest와 puppeteer로 테스트 없는 곳에 테스트 나게 할 수 있을까

<!--break-->
## index 
--- 
- [index](#index)
- [문제](#문제)
- [풀이](#풀이)
- [recap](#recap)

<br>
<br>
<br>

## 문제 
--- 

1. 의존성 설치
2. config
3. 기본적인 테스트

<br>
<br>

## 풀이 
--- 

1. 의존성을 설치하자.

의존성은 언제나 최소한으로 설치하는게 가장 이해하기 좋다.

> 위치: project root
> package.json 파일
```json
{
    ...
  "scripts": {
    "test": "jest"
  },
  "devDependencies": {
    "jest": "^29.4.3",
    "jest-puppeteer": "^7.0.1",
    "puppeteer": "^19.7.2"
  },
  ...
}
```

node 버전은 높을수록 보통 좋다.

2. config

> 위치: project root
> jest.config.js 파일
```js
/** @type {import('jest').Config} */
const config = {
  verbose: true,
  preset: "jest-puppeteer",
  testMatch: [
    "**/test/**/*.test.js"
  ],
};

module.exports = config;
```


3. 기본적인 테스트
```js
// <ROOT>/src/test/ourFirst.test.js
describe('Google', () => {
  beforeAll(async () => {
    await page.goto('https://google.com');
  });

  it('should be titled "Google"', async () => {
    await expect(page.title()).resolves.toMatch('Google');
  });
});
```

만약 잘 안된다면 jest-cli를 글로벌로 설치

```bash
$ npm install jest-cli global
```

당신의 회사 프로젝트 레거시에 TDD를 도입하고 싶지만
테스트가 사막의 풀뿌리만큼만 있다면

E2E부터 시작하는 것도 나쁘지 않을 수 있습니다.


<br>
<br>

## recap 
--- 


<br>
<br>
