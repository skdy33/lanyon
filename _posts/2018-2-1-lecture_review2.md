---
layout: post
title: Review of a lecture 2 - A Diversity-Promoting Objective Function for Neural Conversation Models
---

# summary 1
|Idx|Contents|
|---|---------------------------------------------------------------------|
|Topic|We suggest that the traditional objective function, i.e., the likelihood of output (response) given input (message) is unsuited to response generation tasks. Instead we propose using **Maximum Mutual Information (MMI)** as the objective function in neural models.|
|Dataset|Twitter Conversation Triple Dataset, OpenSubtitles dataset, |
|Github|[Torch implementation](hthttps://github.com/jiweil/Neural-Dialogue-Generation)|
|Conclusion|We show that use of MMI results in a clear decrease in the proportion of generic response sequences, generating correspondingly more varied and interesting outputs|
|Analysis 1|![image 1](https://i.imgur.com/HQWlVHV.png)|

## Problem
* In part at least, this behavior can be ascribed
to the relative frequency of generic responses like
I don’t know in conversational datasets, in contrast
with the relative sparsity of more contentful alternative
responses

## Intuition
*  Intuitively,
it seems desirable to take into account not
only the dependency of responses on messages, but
also the inverse, the likelihood that a message will
be provided to a given response.s

## MMI Criterion
* $\frac{p(S,T)}{p(S)p(T)} = argmax\{ logp(T|S) - logp(T)\}$
* Current research only used the criterion in testing time
  - reason 1 : nontrivial to calculate it
  - reason 2 : time consuming

### generalization of MMI
$argmax\{ logp(T|S) - \lambda logp(T)\} = argmax\{ (1- \lambda ) log(T|S) + \lambda logp(S|T) - \lambda logp(S)\}$

### MMI-antiLM
* $logp(T|S) - \lambda logp(T)$
* limit : ungrammatical responses
  - It penalizes not
only high-frequency, generic responses, but also fluent
ones and thus can lead to ungrammatical outputs
* solution : we replace the language model p(T) with U(T)
  - $U(T) = \prod p(t_k|t_1,...,t_{k-1})*g(k)$ where $g(k) = 1$ if $k \leq \gamma$ $g(k) = 0$
  - intuition : Thanks to memory-loss property of seq-2-seq models, first few sentences determine the decoded sentences. Therefore, penalizing only those is valid.

### MMI-bidi
* $(1-\lambda)logp(T|S) + \lambda logp(S|T)$
* limit : decoding intractable
  - Reason's simple. We literally can't compute $logp(S|T)$
* solution : Pick N-best lists from $logp(T|S)$ and compute $log(S|T)$

## Decoding techniques

### MMI-antiLM
- length of the sequences is Important
  - Score(T) = $p(T|S) - \lambda U(T) + \gamma U(T)$
* hyperparams $\gamma$ and $\lambda$
  - optimize by MERT with N-best lists by beam search.

### MMI-bidi
* hyperparams $\gamma$ and $\lambda$
  - optimize by MERT with N-best lists by beam search.
