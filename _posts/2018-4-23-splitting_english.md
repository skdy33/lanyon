---
layout: post
title: English sentence splitting that has no punctuation
---

## 0. 목적 
* 영어 음성인식 결과물을 문장별로 끊어내야 한다.
* 한글은 그 어미가 상대적으로 명확한데에 비해, 영어는 너무나도 어렵다.
* 사실 자막 자체는 문장으로 끊지 않아도 상관이 없지만, 번역을 위해서는 문장별로 끊는 것이 너무나도 필요하다.
* 그러면 이런 과정에서는, 문장을 만들고, 문장을 다듬고, 문장을 번역하고, 그 다음에 문장을 끊는게 오히려 더 정확할 것 같다. 

## 1. candidates
1. 자체 deep learning 모델 개발
2. 현존하는 ssplit 기술 찾기
3. dependency parsing????


## 2. dependency parsing
* nltk stanford parsing으로 진행해본다.
* 그 다음에는 ```spacy``` 로 진행해본다.

## 3. EnableAutoPunctuation
* googleapi 자체 config auto punct 사용해본다. 개잘된다!!!!
