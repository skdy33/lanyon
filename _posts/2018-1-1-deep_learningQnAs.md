---
layout: post
title: 딥러닝 모델링 QnA
---

## 1. Softmax vs log_softmax

## 2. why we do data normalization
이유는 바로 **learning rate** 때문이다. <br>

gradient를 계산하여 learning rate를 통해 학습하는데, <br>
각 피처마다 학습하는 상대적인 스케일이 절대적인 양의 통일 때문에 같아진다면, <br>
결국 학습이 잘 안되는 것이다. <br>
따라서 normalization을 해야한다.

## 3. logit 이란

## 참고자료
- [why we do data normalization](https://www.researchgate.net/post/When_and_why_do_we_need_data_normalization)
- [data normalization in deep learning](https://stats.stackexchange.com/questions/185853/why-do-we-need-to-normalize-the-images-before-we-put-them-into-cnn)
* [softmax vs log softmax](https://www.quora.com/What-is-the-difference-between-logsoftmax-and-softmax-transfer-function-layer)
* [logit in tensorflow](https://stackoverflow.com/questions/41455101/what-is-the-meaning-of-the-word-logits-in-tensorflow)
