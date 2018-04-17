---
layout: post
title: 논문리뷰2_The_Behavior_of_stock_market_prices
---

# summary 1
|Idx|Contents|
|---|---------------------------------------------------------------------|
|Topic|The purpose of this paper will be to discuss first in more detail the theory underlying the random-walk model and then to test the model's empirical validity.|
|Dataset|TREC dataset, movie reviews dataset, Twitter sentiment dataset|
|Github|[Keras implementation](https://github.com/FredericGodin/DynamicCNN)|
|Conclusion|We test the DCNN in four experiments: small scale binary and multi-class sentiment prediction, six-way question classification and Twitter sentiment prediction by distant supervision. The network achieves excellent performance in the first three tasks and a greater than 25% error reduction in the last task with respect to the strongest baseline.|
|Analysis 1|![image 1](https://i.imgur.com/bL9fhmH.png) <br> Accuracy of sentiment prediction in the movie reviews dataset <br> The first four results are reported from Socher et al. (3013b). <br> The baselines **NB** and **BINB** are Naive Bayes classifiers with, respectively, unigram features and unigram and bigram features. <br> **SVM** is a support vector machine with unigram and bigram features. <br> **RECNTN** is a recursive neural network with tensor-based feature function, which relies on external structural features given by a parse tree and performs best among the RecNNs.|

# 내가 읽으면서 도움을 받을 수 있는 것들.
* random-walk 모델에 대한 간략한 설명 + 디테일 설명ㅁㄴㅇㄹㅁ
