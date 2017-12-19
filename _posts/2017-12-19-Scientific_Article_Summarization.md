---
layout: post
title: summarzation article 1
---

# Scientific Article Summarization Using Citation-Context and Articles's Discourse Structure
* Arman Cohan
* Nazli Goharian

## Abstract
* We propose a summarization approach for scientific articles which takes advantage of **citation-context** and the **document discourse model.**
* Our method overcomes the problem of inconsistency between the citation summary and the article's content **by providing context for each citation.**
* We show that our proposed method effectively improves over existing summarization approaches in terms of ROUGE scores on **TAC2014 scientific summarization dataset.**

## Introduction
* **Scientific summarization is different than general summarization in three main aspects**
  - First, **the length of scientific papers are usually much longer** than general articles
  - Second, in scientific summarization, **the goal is typically to provide a technical summary of the paper which includes important findings, contributions or impacts of a paper to the community.**
  - Finally, scientific papers follow a **natual discourse**.
* There are currently two types of approaches towards scientific summarization.
  - articles' abstracts, citation based summaries.
* The **problem of inconsistency** between the degree of certainty of expressing findings between the citing article and referenced article has been also reported.
* We call the **textual spans** in the reference articles that reflect the citation, the citation-context.
* We extract citation-context in the refernce article for each citation.

## The summarization approach
* Our scientific summary geneartion algorithm is composed of four steps
  - Extracting citation-context
  - Grouping citation-contexts
  - Ranking the sentences within each group
  - Selecting the sentences for final summary
- We assume that the citation text in each citing article is already known.
- We tokenized the articles' text to sentences by using the punkt unsupervised sentence boundary detection algorithm

### 3.1 Extracting the citation-context
* **To find citation-contexts, we consider each citation as an n-gram vector and use vector space model for locating the relevant text spans in the reference article.**
* The similarity function is the **cosine similarity** between the pivoted normalized vectors.
* We used Metamap for extracting biomedical concepts from the citation text
