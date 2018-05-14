---
layout: post
title: react 공부
---

## 0. 목적
react 의 convention을 따르고, react에서 공부한 여러가지를 정리하는 파일이다.

## 1. container
* container는 쉽게 '페이지'라고 생각하면 된다.
* container가 redux랑 대화한다.
* redux(state)랑 대화하다 보니까, class형 컴포넌트로 작성해야 한다.


## 2. component
* component 는 함수형 컴포넌트로 작성하는게 빠르고 편해용

## 3. 함수형, 클래스형 컴포넌트
* 라이프사이클API, state사용을 할 지 안할지로 판단한다.
	* 그렇기 때문에 component, container를 나눌 수 있는 것이다.

## 4. thunk vs saga
* 현재 이해하기론 절차적 프로세스를 하기 위해서 thunk, saga를 쓴다.
* 여기서 thunk와 saga의 차이는, thunk는 콜백을 쓰고 saga는 yield, call을 써서 콜백을 안쓴다는점?


## 참고자료
* [함수형 컴포넌트](https://velopert.com/2994)
* [redux saga](https://rhostem.github.io/posts/2017-09-07-redux-saga-toast-control/)
