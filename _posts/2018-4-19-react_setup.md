---
layout: post
title: create-react-app 을 통한 셋업
---

## 0. 설치

```bash
npm install -g create-react-app
```

## 1. 앱 생성 및 package 설치

```bash
create-react-app 뭣이중헌디
npm install --save axios react-addons-update react-router react-timeago redux react-redux nodemon redux-saga
```

```bash
npm install --save react-router-dom
npm install --save cross-env 
```
* react-router-dom: 브라우저에서 사용되는 리액트 라우터
* cross-env: 프로젝트에서 NODE_PATH를 사용하여 절대경로로 파일을 불러오기 위한 환경변수 설정. 운영체제마다 달라서 꼭 해줘야 한다.

### 1.0 NODE_ENV 설정
* package.json
```js
  "scripts": {
    "start": "cross-env NODE_PATH=src react-scripts start",
    "build": "cross-env NODE_PATH=src react-scripts build",
```

### 1.1 webpack css-loader, style loader
* 이걸 써야 import css가 가능하다. <br>
```bash
npm install --save-dev style-loader css-loader
```

### 1.2 webpack에 로더 적용
* 일단 eject
```bash
npm run eject
```

### 1.3 style 적용
* src/style.css
```css
body {
	    background-color: #ECEFF1;
}
```

* webpack.config.prod.js & webpack.config.dev.js
```js
entry:[
	'./style.css'
]
```

## 기타
* axios: HTTP 클라이언트

* react-addons-update: Immutability Helper (Redux 의 store 값을 변경 할 때 사용됨)

* react-router: 클라이언트사이드 라우터

* react-timeago: 3 seconds ago, 3 minutes ago 이런식으로 시간을 계산해서 몇분전인지 나타내주는 React 컴포넌트

* redux, react-redux; FLUX 구현체, 그리고 뷰 레이어 바인딩

* redux-thunk: redux의 action creator에서 함수를 반환 할 수 있게 해주는 redux 미들웨어, 비동기작업을 처리 할 때 사용됩니다
* path는 의존경로를 절대경로로 변환해주는 모듈입니다.

* nodemon은 코드에 변화가 있을 때 서버를 재시작 해주는 모듈입니다.

## 참고자료
* [갓-velopert](https://velopert.com/2037)
* [Getting started with create-react-app, Redux, React Router & Redux Thunk](https://medium.com/@notrab/getting-started-with-create-react-app-redux-react-router-redux-thunk-d6a19259f71f)
* [redux-saga](https://mskims.github.io/redux-saga-in-korean/)
* [redux-thunk vs redux-saga](https://orezytivarg.github.io/from-redux-thunk-to-sagas/)
* [react-router 사용해보기](https://velopert.com/3417)
