---
layout: post
title: react-redux-saga 분석
---

### [React boilerplate](https://github.com/react-boilerplate/react-boilerplate) 를 기반으로 한 사용처 
# 난 다시는 react boilerplate를 사용하지 않을 것이다.(앞으로는 create-react-app 사용)

## 1. 구성

* React Router
* Redux
* Redux Saga
* [Reselect](https://orezytivarg.github.io/improving-react-and-redux-performance-with-reselect/)
* immutableJS
* 등등등

### 1.1 [container/component architecture](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)
* [example](https://gist.github.com/chantastic/fc9e3853464dffdb1e3c)

#### 1.1.1 Container - 똑똑한 컴포넌트
* Are concerned with how things work.
* Are usually generated using higher order components such as connect() from React Redux, createContainer() from Relay, or Container.create() from Flux Utils, rather than written by hand.

#### 1.1.2 component - 멍청한 컴포넌트
* Are concerned with how things look
* Are written as functional components unless they need state, lifecycle hooks, or performance optimizations

## 2. saga
* saga는 결국 reducer가 또다른 reducer를 부르고, 이런 형식의 코드를 짜기 위해서 필요한 것 같다.
* redux-thunk는 Action Creator가 함수를 넘겨주기에 필연적으로 Action Creator에 비동기처리의 코드나 관련된 로직이 (약간 무리하게) 들어갑니다. 반면, redux-saga는 비동기 처리를 기술하는 전용의 방식인 Task로 쓰여있습니다(참고자료 복붙)

### actions
* actionTypes
	* 진짜 말 그대로 상수 하나 지정해놓고, 리듀서는 특정 actiontype에따라 어떻게 state를 바꿀 지 보고있고, saga는 actiontype에 따라 어떤 call을 할 지 보고있다.
* action function
	* 액션 함수는 실제로 어떤 parameter를 받을 것인지, 해당 함수는 어떤 actiontype으로 지정되는지만을 정한다.
	* 이 액션 펑션을 container에서 연결(connect)해주는 것이다.
* reducer
	* reducer에서는 어떠한 actiontype이 일어났을 때, store에 뭘 저장할지만을 규정한다.
	* action을 인자로 받는데, action. 으로 접근할 수 있는 파라미터는 우리가 action function에서 정의했다.
* saga
	* saga index에서는 일단, saga 함수와 actiontype을 연결한다. 
	* 그리고 각 saga generator function에서는 그 actiontype에서 받는 인자를 action으로 받아서, 어떤 프로세스로 비동기처리가 될 것인지 짠다.


## QnA
* path(['data'],response) 와 response.data의 차이는?
### 2.1 generator function
* 나갔다가 다시 돌아올 수 있는 함수.
* Generator 함수는 호출되어도 즉시 실행되지 않고, 대신 함수를 위한 Iterator 객체가 반환.
* 반환된 객체는 next()가 yield를 만날 때까지 돌아감.
* .next() 로 반환된 객체는 .value와 .done의 객체를 가진다.
### 참고자료
* [redux-saga로 비동기처리와 분투하다](https://github.com/reactkr/learn-react-in-korean/blob/master/translated/deal-with-async-process-by-redux-saga.md)
* [saga tutorial](https://redux-saga.js.org/docs/introduction/BeginnerTutorial.html)
* [generator function](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/function*)
