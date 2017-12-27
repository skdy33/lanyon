---
layout: post
title: 논문리뷰1_one_shot_learning_with_memory_augmented_networks
---

#[딥마인드의 one shot learning](http://getliner.com/webpdf/web/viewer.html?file=09457d5ccba210993f1a1746acfd50099556a855.pdf)

## 0. Abstract
* Architectures with augmented memory augmented memory capacities, such as Neural Turing Machines, offer the ability to quickly encode and retrieve new information, and hence can potentially obviate the downsides of conventional models.
  - Downside : When new data is encountered, the models must inefficiently relearn their parameters to adequately incorporate the new information without catastrophic interference.

## 1. Introduction
* However, previous work does suggest one potential strategy for attaining rapid learning from sparse data, and hinges on the notion of meta-learning(Lifelong learning algorithms.  In
Learning to learn, pp. 181–209. Springer, 1998)
* **"learning to learn"** -  Rapid learning occurs
within a task,  for example, when learning to accurately classify within a particular  dataset. This  learning  is  guided  by  knowledge accrued  more  gradually across tasks,  which  captures  the way in which task structure varies across target domains. Given its two-tiered organization, this form of meta-learning is often described as *learning to learn*
* A scalable solution has a few necessary requirements
  - First, information must be stored in memory in a representation that is both stable (so that it can be reliably accessed when needed) and element-wise addressable (so that relevant pieces of information can be accessed selectively)
  - Second, the number of parameters should not be tied to the size of the memory
* We demonstrate that MANNs are capable of meta-learning
in tasks that carry significant short- and long-term mem-
ory  demands

## 2. Meta-Learning Task Methodology
### [meta learning에 대한 전반적 설명](http://bair.berkeley.edu/blog/2017/07/18/learning-to-learn/)

* $\theta^{*} = argmin_{\theta}E_{D~p(D)}[L(D;\theta)]$
  - For meta learning, we choose parameters to reduce the expected learning cost across a distribution of datasets p(D)
* Importantly, labels are shuffled from datset-to-dataset.
  * This prevents the network from slowly learning sample-class bindings in its weights.
* Ultimately, the system aims at modelling the predictive distribution $p(y_t | x_t, D_{1:t-1};\theta)$, inducing a corresponding loss at each time step.

### notation
* $p(D)$ : datasets
* $D = {d_t}^T_{t=1} = \{(x_t,y_t)\}^T_{t=1}$ : some datasets
* $y_t$ : For classification, class label for an image $x_t$. for regression, $y_t$ is the value of a hidden function for a vector with real-valued elements $x_t$



## 비고
* meta-learning같은건, 그 abstract 내지는 내용 요약이라도 좀 보이면 좋을 것 같다.
* 대명사는 그 대명사의 설명이 꼭 필요하다. 예) this method compensates for the problem.
* 쌍따옴표 등으로 표시된 키워드는 진짜 키워드. 해당 내용이 들어있으면 좋을 것 같다.
* However 다음은 굉장히 중요
* 수사
* We 다음은 주장
* 수식과 그에 대한 설명
* 각 notation에 대한 설명
* Importantly, labels are shuffled from datset-to-dataset.
  - 이런 문장의 경우에는 전체가 보여야 할 것 같다
