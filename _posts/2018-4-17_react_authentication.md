---
layout: post
title: react_redux_saga_django-rest-framework_로그인_구현
---

## 0. 목적
현재 **wide open** 인 우리 웹(react 기반)을 로그인 해서 로그인 한 사람한테 할당된 영상만 올릴 수 있도록 한다. <br>
1. django-rest-framework에서 token based authentication을 구현 - 여기까지는 완료.<br>
*[제 다른 글을 찾아보세용](https://skdy33.github.io/2018/04/17/Django_rest_framework_auth/)*
2. 로그인 페이지를 만들어서, 로그인을 하면 메인으로 갈 수 있도록 한다.
	* 회원가입 페이지는 노 필요 아직.


## 사전작업
* ```npm install --save-dev style-loader css-loader```
* html에 Materializecss 불러오기.

## 모듈
* axios: HTTP 클라이언트
* react-addons-update: Immutability Helper (Redux 의 store 값을 변경 할 때 사용됨)
* react-router: 클라이언트사이드 라우터
* react-timeago: 3 seconds ago, 3 minutes ago 이런식으로 시간을 계산해서 몇분전인지 나타내주는 React 컴포넌트
* redux, react-redux; FLUX 구현체, 그리고 뷰 레이어 바인딩
* redux-thunk: redux의 action creator에서 함수를 반환 할 수 있게 해주는 redux 미들웨어, 비동기작업을 처리 할 때 사용됩니다

* **근데 우리는 ```redux-thunk``` 말고 saga를 사용해보도록 한다.

## css-loader, style-loader 설치
* ```npm install --save-dev style-loader css-loader```
* 본 부분에 대한 적용은 잘 모르겠다....
	* [velopert](https://velopert.com/1954)의 페이지를 참고한다.

## Materializecss index.html에 추가
```
<!DOCTYPE html>
<html>

   <head>
      <meta charset="UTF-8">
      <!--Import Google Icon Font-->
      <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
      <!--Import materialize.css-->
      <link type="text/css" rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/0.97.6/css/materialize.min.css"  media="screen,projection"/ />
      <!--Let browser know website is optimized for mobile-->
      <meta name="viewport" content="width=device-width, initial-scale=1.0"/>

      <title>MEMOPAD</title>
   </head>

   <body>
      <div id="root"></div>
      <script type="text/javascript" src="https://code.jquery.com/jquery-2.1.1.min.js"></script>
      <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/materialize/0.97.6/js/materialize.min.js"></script>
      <script src="/bundle.js"></script>
   </body>

</html>
```

## 1. 로그인 페이지
1. 일단 rc snippet을 통하여 component 생성.

```js
import React, { Component } from 'react';
import PropTypes from 'prop-types';

const propTypes = {
  mode: React.PropTypes.bool,
  onLogin: React.PropTypes.func,
  onRegister: React.PropTypes.func
};
const defaultProps = {
  mode: true,
  onLogin: (id, pw) => { console.error("login function not defined"); },
  onRegister: (id, pw) => { console.error("register function not defined"); }
};

class Authentication extends Component {
    constructor(props) {
        super(props);
    }
    render() {
        return(
            <div>Authentication</div>
        );
    }
}
Authentication.propTypes = propTypes;
Authentication.defaultProps = defaultProps;
export default Authentication;
```
2.
3. 해당 component 렌더링 하는 container 생성

## ps
* snippet

```cson
'.source.js':
    'React Component':
        'prefix': 'rc'
        'body': '''
        import React, { Component } from 'react';
        import PropTypes from 'prop-types';

        const propTypes = {
        };
        const defaultProps = {
        };
        class ${1:MyComponent} extends Component {
            constructor(props) {
                super(props);
            }
            render() {
                return(
                    <div>${1:MyComponent}</div>
                );
            }
        }
        ${1:MyComponent}.propTypes = propTypes;
        ${1:MyComponent}.defaultProps = defaultProps;
        export default ${1:MyComponent};
        '''

```

## 자 근데 이제 localStorage and redux store
* 지금은 새로고침 할 때마다 store가 없어지니까, 꺼진다
* 따라서 localStorage에 token을 저장해야 하는데,
* 지금 store에 subscribe이 있다는데?????


## 궁금증
* history란?
	* history에서 signUp 같은걸 push하는데....
* header.js 의 역할이란?
	* 진짜 그냥 페이지별 대가리
* reducer 란?
	* 그니까 redux는 어플리케이션 전반의 state를 관리하기 위한 event loop라고 생각을 했을 때
	* reducer는 action(특정 event)에 대한 반응을 이야기하는 것이다.
	* 그렇기 때문에, redux는 reducer와 action으로 이루어진 것이다.
## 참고자료
* [velopert 로그인 페이지 만들기](https://velopert.com/1967)
* [velopert 짱](https://velopert.com/1954)
* [함수형 컴포넌트](https://velopert.com/2994)
* [saga beginner tutorial](https://redux-saga.js.org/docs/introduction/BeginnerTutorial.html)
* [saga 한국말!](https://mskims.github.io/redux-saga-in-korean/)
