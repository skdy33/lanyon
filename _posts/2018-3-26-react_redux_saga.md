---
layout: post
title: react-redux-saga 분석
---

# [React boilerplate](https://github.com/react-boilerplate/react-boilerplate) 를 기반으로 한 사용처 

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
