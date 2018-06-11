---
layout: post
title: canvas + react
---

## why canvas
* 내가 보여줄 일련의 이미지를 canvas라는 박스 안에 넣어야 하는 것 같다.
* \<canvas\> 요소는 고정된 크기의 드로잉 영역을 생성하는데, 그 영역은 보여질 컨텐츠를 생성하고 다루게될 두가지 이상의 렌더링 컨텍스트를 노출시킵니다
* 근데 사실 내가 사용할 wavesurf 그 자체는 canvas를 포함하고 있다.
* 따라서 wavesurf와 함께 동작할 box container 를 위한 작업이 될 수 있다

## 요소들
* clientX, layerX, movementX, offsetX, pageX 많은데
* offsetX 가 top left canvas 대비 그 위치이다.

## 참고자료
* [캔버스 기본 사용법](https://developer.mozilla.org/ko/docs/Web/HTML/Canvas/Tutorial/Basic_usage)
* [react + \<canvas\>](https://blog.lavrton.com/using-react-with-html5-canvas-871d07d8d753)
